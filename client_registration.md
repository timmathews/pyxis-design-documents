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
  "device_type": "tablet",
  "connection": "wireless",
  "data": ["speed", "direction", "location", "temperature"]
}
```
