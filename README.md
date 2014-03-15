Pyxis Design Documents
======================

Design guidance and for Pyxis

Structure of this document will evolve over time.

Building screens should follow a drag-n-drop paradigm, the toolbox will include (at least) the following widgets:
 * analog gauge
 * digital gauge
 * bar gauge (horizontal or vertical)
 * warning lamp
 * toggle switch
 * slider
 * compass card
 * video player
 * text
 * image
 * histogram chart
 * line chart
 * bar chart
 * sparkline chart

We are *not* looking to build a WYSIWYG HTML editor, so complex screens will need to be built by hand in "editor of choice."

Should be themeable by writing CSS, a CSS class guide will need to be developed.

Assigning data to gauges should be done with context modal boxes which should appear when the control is clicked in 'edit' mode. This mode will also allow for setting data ranges, warning levels, etc.

Every datum will have default ranges (min, max, under-warn, under-danger, over-warn, over-danger) and thresholds for OK, warning, danger triggering. These levels can be overridden on a per-display (even per-control) basis. In this way, it becomes simple to show a large range value (e.g. apparent wind) and a detail view (e.g. close-hauled wind) side-by-side using the same data stream. Damping and averaging will also have server defaults and per-device settings.

The "how" of this is not defined yet. Likely we will store the configuration on the local device as XML or something.

Data Streams
------------

Data streams are encoded with [MessagePack](https://github.com/msgpack/msgpack) and pushed around by ZeroMQ in a Pub-Sub configuration with topics. Naturally, web clients will connect with WebSockets. There should be a topic per PGN, but also topics for common groups of PGNs, topics for synthetic data that doesn't exist as a NMEA 2000 PGN, and potentially custom topics.
