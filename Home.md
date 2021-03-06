# What is EDDN?

The **Elite: Dangerous Data Network** is a system for willing Commanders to share dynamic data about the galaxy with others. By pooling data in a common format, tools and analyses can be produced that add an even greater depth and vibrancy to the in-game universe.

EDDN is not run by or affiliated with [Frontier Developments](http://www.frontier.co.uk/). Hosting has been very generously provided by [Vivio Technologies](https://www.viviotech.net/).

# Using EDDN

EDDN is currently in a proof-of-concept phase. Any information here may be subject to change, but this page should be mostly up to date.

How you use EDDN depends on who you are:

## Trading software users

Ask the developer of your software to incorporate support for EDDN! When your software supports using the EDDN, you'll have access to the data of all other Commanders who are participating - and you'll be helping keep the network running.

## Trading software developers

To upload market data to EDDN, you'll need to make a POST request to the URL:

* http://eddn-gateway.elite-markets.net:8080/upload/

The body of the request should be a JSON-format message containing the market data. 

The exact format specification is still under development, but will be along the lines of the following:

    {
        "$schemaRef": "http://schemas.elite-markets.net/eddn/commodity/1",
        "header": {
            "uploaderID": "abcdef0123456789",
            "softwareName": "My Awesome Market Uploader",
            "softwareVersion": "v3.14"
        },
        "message": {
            "systemName": "Eranin",
            "stationName": "Azeban Orbital",
            "itemName": "Gold",
            "buyPrice": 1024,
            "stationStock": 7,
            "sellPrice": 1138,
            "demand": 42,
            "timestamp": "2014-11-17T12:34:56+00:00"
        }
    }

This is a very similar format to that used by the short-lived EMDN service, so existing clients should be able to work with it with only minor modifications. Note that ordering of properties is not considered significant in JSON objects.

It is expected that other types of data will be carried by the EDDN; the special '$schemaRef' property of the message can be used to determine what sort of message is being sent, and what version of the format spec is being used.

You MUST include a '$schemaRef' property in the root object of your message. The $schemaRef MUST be one of the following:
* http://schemas.elite-markets.net/eddn/commodity/1 for commodity price data messages
* http://schemas.elite-markets.net/eddn/commodity/1/test for testing commodity price data messages
* http://example.com for testing arbitrary JSON - no further validation is applied

## Other developers (data users)

EDDN provides a continuous stream of information from uploaders. To use this data you'll need to connect to the stream using ZeroMQ (a library is probably available for your language of choice).

Currently available relays:
* tcp://eddn-relay.elite-markets.net:9500

You'll need to use your ZeroMQ library to connect to that stream, then zlib-decompress (as simple as zlib.decompress(message) in Python!) the messages as they come over the stream. And that's all - you then have access to all the data being uploaded to the network, almost instantly.

Consumers of data SHOULD examine the $schemaRef property to determine what kind of message has been received, and what if any processing to carry out on the data.

A sample client application is at https://github.com/jamesremuscat/EDDN/blob/master/src/eddn/Client.py - it simply dumps the data to the console. You'll probably want to do something more exciting with it!

# Contact

* [E:D forum thread](https://forums.frontier.co.uk/showthread.php?t=57986)
* [E-mail bugs@elite-markets.net](mailto:bugs@elite-markets.net)