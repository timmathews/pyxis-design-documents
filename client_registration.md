When a new client connects to the pyxis server, it sends some basic information:
* protocol version
* name
* device type: phone / tablet / desktop computer / instrument display / etc
* connection: wired or wireless
and a list of data points that it wants to receive. Changing this list should be possible without restarting the connection.

The JSON representation of this data is something like this:
```JavaScript
{ "version": "1.2",
  "name": "Nav Station iPad",
  "data_format": "json",
  "data": ["speed", "direction", "location", "temperature"]
}
```

The registration message fields are:
* ```version```: required, string, represents the API version the client understands
* ```name```: optional, string, the name of the client device. Can be any UTF-8 string, limited to 120 characters
* ```data_format```: optional, string, either json or msgpack. If omitted, msgpack is assumed
* ```data```: required, array of message identifiers. These identifiers can be discovered by the client. Implementation details here have not been determined. These may also be sent as an array of objects. See below.

```JavaScript
{ "version": "1.2",
  "name": "Nav Station iPad",
  "data_format": "json",
  "data": [
    { "field": "speed",
      "rate": 10,
      "damp": 100
    },
    { "field": "direction",
      "rate": 2,
      "damp": 500
    }
  ]
}
```

In this case, we have defined a rate in Hertz and damping period in tenths of a second for each field. So in the first case, we are telling the server to send us the speed datum averaged over all readings of the last 100ms ten times per second. The second field asks for the direction datum at a rate of 2Hz and averaged over 500ms.

Future versions may move the version, name and data_format fields to HTTP headers.

The server will respond to this with

```JavaScript
{ "session": "12ade39d2fd4ea20342d03d20d29d2",
  "data": [
    { "field": "speed",
      "title": "Speed Over Ground",
      "abbr": "SOG",
      "units": "kts",
      "multiplier": 0.01,
      "rate": 10,
      "damp": 100,
      "min": 0,
      "max": 14
    },
    { "field": "direction",
      "title": "Magnetic Compass Heading",
      "abbr": "HDGm",
      "units": "deg",
      "multiplier": 0.01,
      "rate": 2,
      "damp": 500,
      "min": 0,
      "max": 359
    }
  ]
}
```

* ```session``` uniquely identifies the session for the client and is used later as a unique "topic" to subscribe to.
* ```data``` describes each field that the client requested in detail. This can be used to draw gauges. The attributes shown above are not an exhaustive list. Other possibilities include an array of values indicating colored ranges on the gauges to show for instance, low values, OK values and dangerous values. They may also indicate related data or in the case of multi-valued data, subfields.
 
In the control channel where the client connects and communicates changes to the server all communication is done in JSON. However, the data channel packets can be in JSON or MessagePack at the discretion of the client. Other formatting may be supported later provided there exists a decent encoding package for Go.
