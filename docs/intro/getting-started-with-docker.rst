Getting Started with Docker
===========================

The simplest way to get started with RTB4FREE is to use Docker to install all components
onto a single system, so that you can experience the functionality and quickly see
if RTB4FREE is right for your business.

.. note::
  All services are installed on a single machine in this installation method.  This
  is not recommended for production use - only for demonstration purposes.


* Starts a complete RTB system, along with a campaign manager and reporting system.
* A demo campaign configuration is loaded and can be edited in the campaign manager.
* An SSP exchange simulator is started, sending bid requests to the system. This shows a working RTB application processing real bids.
* View how data flows through the system in real-time with the deployed reporting system.

.. _extensions: http://www.sphinx-doc.org/en/master/ext/builtins.html#builtin-sphinx-extensions


.. _quick-start-video:

Quick start video
-----------------

This screencast will help you get started or you can
:ref:`read our guide below <quick-start>`.

.. raw:: html

    <div style="text-align: center; margin-bottom: 2em;">
    <iframe width="100%" height="350" src="https://www.youtube.com/embed/oJsUvBQyHBs?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
    </div>


.. _quick-start:

Quick start
-----------

This quick start process will deploy a Docker swarm application.
You must have Docker installed on your system before you start - https://docs.docker.com/v17.12/install/

Create the Docker swarm.

.. prompt:: bash $

    docker swarm init

Create the docker compose file. Edit a new file called docker-compose-rtb4free.yml, then add the following contents.

.. prompt:: bash $

    version: "3"
    services:
      zookeeper:
        image: "zookeeper"
      kafka:
        image: "ches/kafka"
        environment:
          ZOOKEEPER_IP: "zookeeper"
        ports:
          - "9092:9092"
        depends_on:
          - zookeeper
      zerospike:
        image: "jacamars/zerospike:v1"
        environment:
          BROKERLIST: "kafka:9092"
        depends_on:
          - kafka
        command: bash -c "sleep 5 && ./wait-for-it.sh kafka:9092 -t 120 && sleep 1; ./zerospike"
      bidder:
        image: "jacamars/rtb4free:v1"
        environment:
          BROKERLIST: "kafka:9092"
          PUBSUB: "zerospike"
          EXTERNAL: "http://localhost"
          ADMINPORT: "0"
          ACCOUNTING: "accountingsystem"
          FREQGOV: "false"
          INDEXPAGE: "/index.html"
        ports:
          - "80:8080"
          - "8155:8155"
        depends_on:
          - kafka
          - zerospike
        command: bash -c "sleep 5 && ./wait-for-it.sh kafka:9092 -t 120 && ./wait-for-it.sh zerospike:6000 -t 120 && sleep 1; ./rtb4free"
      crosstalk:
        image: "jacamars/crosstalk:v1"
        environment:
          REGION: "US"
          GHOST: "elastic1"
          AHOST: "elastic1"
          BROKERLIST: "kafka:9092"
          PUBSUB: "zerospike"
          CONTROL: "8100"
          JDBC: "jdbc:mysql://db/rtb4free?user=ben&password=test"
          PASSWORD: "iamspartacus"
        depends_on:
          - kafka
          - zerospike
      db:
        image: ploh/mysqlrtb
        environment:
          - MYSQL_ROOT_PASSWORD=rtb4free
          - MYSQL_DATABASE=rtb4free
          - MYSQL_USER=ben
          - MYSQL_PASSWORD=test
      web:
        image: ploh/rtbadmin_open
        command: bash -c "./wait_for_it.sh db:3306 --timeout=120; bundle exec rails s -p 3000 -b '0.0.0.0' -e development"
        ports:
          - "3000:3000"
        environment:
          - CUSTOMER_NAME=RTB4FREE
          - RTB4FREE_DATABASE_HOST=db
          - RTB4FREE_DATABASE_PORT=3306
          - RTB4FREE_DATABASE_USERNAME=ben
          - RTB4FREE_DATABASE_PASSWORD=test
          - RTB4FREE_DATABASE_NAME=rtb4free
          - ELASTICSEARCH_ENABLE=true
          - ELASTICSEARCH_HOST=elastic1:9200
          - ELASTICSEARCH_KIBANA_URL=http://kibana:5601/
          - RTB_CROSSTALK_REGION_HOSTS={"US" => "crosstalk"}
          - RTB_CROSSTALK_PORT=8100
          - RTB_CROSSTALK_USER=ben
          - RTB_CROSSTALK_PASSWORD=iamspartacus
      elastic1:
        image: ploh/elastic_pwd
        environment:
          - discovery.type=single-node
          - "ES_JAVA_OPTS=-Xms1g -Xmx1g"
      logstash1:
        image: ploh/logstash_pwd
        environment:
          - "XPACK_MONITORING_ELASTICSEARCH_URL=http://elastic1:9200"
          - "XPACK_MONITORING_ENABLED=true"
      kibana:
        image: docker.elastic.co/kibana/kibana:6.2.2
        environment:
          - SERVER_NAME=elastic1
          - ELASTICSEARCH_URL=http://elastic1:9200
        ports:
          - "5601:5601"
      simulator:
        image: "jacamars/rtb4free:v1"
        environment:
          BIDDER: "bidder:8080"
          WIN:    "10"
          PIXEL:  "95"
          CLICK:  "2"
          SLEEP:  "100"
        command: bash -c "./wait-for-it.sh bidder:8080 -t 120 && sleep 60;  ./load-elastic -host $$BIDDER -win $$WIN -pixel $$PIXEL -click $$CLICK -sleep $$SLEEP"

Start the docker swarm

.. prompt:: bash $

    docker stack deploy -c docker-compose-rtb4free.yml rtb4free


You should see containers starting each RTB4FREE component.
To show the status, issue the command:

.. prompt:: bash $

    docker stack ps rtb4free

You should see the following response.

.. prompt:: bash $

    xxxx

After the system is started, you can try the following actions.

* Access the campaing manager by opening a browser to URL http://localhost:3000/. You can log in with user ID demo@rtb4free.com, password rtb4free.
	* View how a sample campaign is defined.
	* View how a sample creative is defined.
	* View how a sample target is defined.
	* View the sample reports dashboard.
	* Edit the sample campaigns.

* The application includes the ELK stack (http://elastic.co) for ingesting RTB log events.
	* You can access Kibana reporting system at URL http://localhost:5601.
	* On the discover screem, the index drop down will allow you to show requests, bid and wins processed by the bidder.
	* Build custom visualizations and dashboards using Kibana.

* Explore the internal works of the bidder by logging into the bidder console at http://localhost:8080/admin_login.

.. _this blog post: http://ericholscher.com/blog/2016/mar/15/dont-use-markdown-for-technical-docs/


External resources
------------------

Here are some external resources to help you learn more about RTB4FREE.

* `RTB4FREE documentation`_


.. _RTB4FREE documentation: http://www.rtb4free.com/doc_index
