# What is EDDN?

The **Elite: Dangerous Data Network** is a system for willing Commanders to share dynamic data about the galaxy with others. By pooling data in a common format, tools and analyses can be produced that add an even greater depth and vibrancy to the in-game universe.

EDDN is not run by or affiliated with Frontier Developments.

# Using EDDN

How you use EDDN depends on who you are:

## Trading software users

Bug the developer of your software to incorporate support for EDDN!

## Trading software developers

To upload market data to EDDN, you'll need to make a POST request to the URL:

* http://eddn-gateway.elite-markets.net:8080/upload/

The body of the request should be a JSON-format message containing the market data. The format is along the lines of the following:

`tbc`

## Other developers (data users)

EDDN provides a continuous stream of information from uploaders. To use this data you'll need to connect to the stream using ZeroMQ (a library is probably available for your language of choice).

Currently available relays:
* tcp://eddn-relay.elite-markets.net:9500

You'll need to use your ZeroMQ library to connect to that stream, then zlib-decompress the messages as they come over the stream. And that's all - you then have access to all the data being uploaded to the network, almost instantly.

A sample client application is at https://github.com/jamesremuscat/EDDN/blob/master/src/eddn/Client.py - it simply dumps the data to the console. You'll probably want to do something more exciting with it!