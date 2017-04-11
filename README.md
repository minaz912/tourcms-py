# tourcms

A simple wrapper for connecting to [TourCMS Marketplace API](http://www.tourcms.com/support/api/mp/). This wrapper mirrors the TourCMS PHP library.

[![Build Status](https://travis-ci.org/prio/tourcms.png?branch=master)](https://travis-ci.org/prio/tourcms)

## Install

NOTE: At the time of writing this repo is ahead of the version in pip which does not include the fix for non GMT environments.

    pip install tourcms

## Usage

Using the library is as simple as creating a **Connection** object:

    conn = tourcms.Connection(marketplace_id, private_key, result_type)

Your Marketplace ID and Private Key can be found in the TourCMS Partner Portal. The result type can be one of **dict** or **raw** where **raw** will return the raw XML from the API and **dict** will return a dictionary of the result. Dict requires xmltodict to be installed.

### Working with your connection in Raw mode

```python
    # Instantiate the connection
    import os
    from tourcms import Connection

    conn = Connection(0, os.getenv('TOURCMS_PRIVATE_KEY'))

    # Check we're working
    conn.api_rate_limit_status(channel_id)
    => "<?xml version="1.0" encoding="utf-8" ?><response><request>GET /api/rate_limit_status.xml</request>
        <error>OK</error><remaining_hits>1999</remaining_hits><hourly_limit>2000</hourly_limit></response>"

    # List the channels we have access to
    conn.list_channels(channel_id)
    => ""<?xml version="1.0" encoding="utf-8" ?><response><request>GET /p/channels/list.xml</request>
        <error>OK</error><channel>(...)</channel><channel>(...)</channel><channel>(...)</channel></response>"

    # Show a particular channel
    conn.show_channel(1234567)
    => ""<?xml version="1.0" encoding="utf-8" ?><response><request>GET /p/channels/list.xml</request>
        <error>OK</error><channel>(...)</channel></response>"
```

### Working with your connection in Dictionary mode
Requires xmltodict to be installed

    pip install xmltodict


```python
    # Instantiate the connection
    conn = Connection(0, os.getenv('TOURCMS_PRIVATE_KEY'), "dict")

    # Check we're working
    conn.api_rate_limit_status(channel_id)
    => OrderedDict([(u'request', u'GET /api/rate_limit_status.xml?'), (u'error', u'OK'), (u'remaining_hits', u'1999'), (u'hourly_limit', u'2000')])
    obj["hourly_limit"]
    => 2000
```

### Passing parameters

Many TourCMS methods accept parameters. Most methods take a dictionary of parameters like so:

    obj = conn.search_tours({"country": "GB", "lang": "en"})

## List of functions in tourcms.Connection

*   api\_rate\_limit\_status(channel)
*   list\_channels()
*   show\_channel(channel)
*   search\_tours(channel, params)
*   search\_hotels\_specific(tour_id, channel, params)
*   list\_tours(channel, params)
*   list\_tour\_images(channel, params)
*   show\_tour(tour, channel, params)
*   show\_tour\_departures(tour_id, channel, params)
*   show\_supplier(supplier_id, channel)
*   get\_booking\_redirect\_url(channel, url)
*   list\_tour\_locations(channel, params)
*   list\_product\_filters(channel)
*   show\_tour\_dates\_deals(tour_id ,channel, params)
*   create\_enquiry(channel, params)
*   search\_enquiries(channel, params)
*   show\_enquiry(enquiry_id, channel)
*   tour\_avail(self, tour_id, channel, date, rates)
*   start\_booking(self, booking\_key, customers_no, components, customers, channel)
*   commit\_booking(self, booking\_id, channel)

## Dependencies

None. xmltodict optional. Tested with Python 2.6, 2.7, 3.3 & 3.6.

## Copyright

Copyright (c) 2012 Jonathan Harrington. See LICENSE.txt for further details.
