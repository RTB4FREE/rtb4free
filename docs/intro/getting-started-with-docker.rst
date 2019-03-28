Getting Started with Docker
===========================

The simplest way to get started with RTB4FREE is to use Docker to install all components
onto a single system, so that you can experience the functionality and quickly see
if RTB4FREE is right for your business.

.. note::
  All services are installed on a single machine in this installation method.  This
  is not recommended for production use - only for demonstration purposes.

This quickstart process performs the following tasks:
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

.. literalinclude:: ../../examples/docker-compose.yml
   :language: yaml

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
