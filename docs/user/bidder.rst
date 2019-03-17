Bidder
======

The Bidder engine is the key component of the RTB4FREE platform. It interfaces
directly with the SSP exchanges using OpenRTB protocols or proprietary protocols,
if necessary, such as Google AdX or OpenX.

The bidder is is a Java version 1.8, and supports OpenRTB version 2.5.
The source code with program description can be found here: https://github.com/benmfaul/XRTB

The Bidder features include:

* OpenRTB 2.5 Compliant.
* Supports many SSPs.
* Supports SSL.
* Supports GZIP Compressed Bids.
* Simplified standalone Bidder Management.

The Bidder can operate as a stand-alone system. Campaigns and Exchange interfaces
can be defined in its configuration files - see the github source or the demo instructions
on this site for detail on how to set up these files.

A sample configuration file, stored at Campaigns/payday.json.

.. literalinclude:: ../../examples/payday.json
   :language: javascript
