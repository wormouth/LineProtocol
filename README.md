# LineProtocol

This is a small header-only Arduino/C++ library to format or parse data in the [InfluxDB Line Protocol](https://docs.influxdata.com/influxdb/v2.0/reference/syntax/line-protocol/).
The build system used is platformio.

Code in `LineProtocol` is written to be readable as opposed to optimal
performance wise. If you encounter situations where performance is
an issue, or if you encounter what you feel to be bugs then please file
a ticket at the [LineProtocol repository](https://github.com/wormouth/LineProtocol).

## Format

The InfluxDB Line Protocol looks like this:

```
<measurement>[,<tag_key>=<tag_value>[,<tag_key>=<tag_value>]] <field_key>=<field_value>[,<field_key>=<field_value>] [<timestamp>]
```

Which offers a lot of flexibility for your boards and other applications. The
reason for this library to exist is that speaking a common format over MQTT
allows you to add display boards without writing custom code to understand the
formats you have for each board and to circumvent the step of writing custom code
to reformat boards into the way you like it.

## In Development

This library is still in development hence the `0.x` version scheme. The
following important items still need to be completed before this can move
onto a stable release cycle:

- [ ] Format messages.
- [ ] Support quotes in measurement, tags, and keys.
- [ ] Support parsing the timestamp.
- [ ] Take another look at using exceptions but at the minimum sane errors.
- [x] Run tests natively with `FakeArduino`.

## Usage

### Installation

`LineProtocol` is listed as an installable package on the PlatformIO registry,
you can add `supakeen/LineProtocol` to your `lib_deps` in `platformio.ini` to
use it.

### Parsing

You can parse some data into a `struct line_protocol` like so:

```cpp
#include <LineProtocol.h>

try {
  LineProtocol::Message message("measurement,key=value key=value");
} catch (...) { }

message.measurement;  // Contains the measurement name.

if(message.tags.count("room")) {  // check if a room tag was supplied
  message.tags["room"];  // use it
}


if(message.fields.count("value")) {  // check if a value field was supplied
  message.fields["value"];  // use it
}
```

### Formatting

Unsupported in `0.3.0` but the next feature on the list.

## Testing

To test the library make sure you connect an `esp32` board then run:

```
pio test -e esp32
```

Which will upload firmware and run tests while fetching the results remotely.
Tests can also be ran natively with:

```
pio test -e native
```

Which doesn't require you to connect a board.
