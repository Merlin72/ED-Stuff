# Using EDDN

How you use EDDN depends on who you are:

## Trading software users

Bug the developer of your software to incorporate support for EDDN!

## Trading software developers

To upload market data to EDDN, you'll need to make a POST request to a URL (which is TBC). The body of the request should be a JSON-format message containing the market data. Format is TBC.

## Other developers (data users)

EDDN provides a continuous stream of information from uploaders. To use this data you'll need to connect to the stream using ZeroMQ (a library is probably available for your language of choice).

A sample client application is at https://github.com/jamesremuscat/EDDN/blob/master/src/eddn/Client.py - it simply dumps the data to the console. You'll probably want to do something more exciting with it!