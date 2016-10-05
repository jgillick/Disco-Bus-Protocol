[:arrow_up: Index](#README.md)

# Disco Bus Message Format

All Disco Bus messages follow this format:

<table style="text-align: center;">
  <thead>
    <tr>
      <th colspan="7">Msg Header</th>
      <th>Msg Data</th>
      <th colspan="2">Msg Footer</th>
    </tr>
    <tr>
      <th colspan="2">Start</th>
      <th>Flags</th>
      <th>Addr</th>
      <th>Cmd</th>
      <th colspan="2">Lengths</th>
      <th>Data</th>
      <th colspan="2">Checksum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0xFF</td>
      <td>0xFF</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>...</td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

## Message Header

### Start of message

All messages start with `0xFF 0xFF`.

### Flags

A byte who's combination of bits determine specific characteristics about the message.

| Bit Flag  | Meaning                                                       |
|-----------|---------------------------------------------------------------|
| `0x00`    | Standard message                                              |
| `0x01`    | [Batch message](../docs/communication.md#batch-message)       |
| `0x02`    | [Response message](../docs/communication.md#response-message) |
| `0x04`    | Reserved                                                      |
| `0x08`    | Reserved                                                      |
| `0x10`    | Reserved                                                      |
| `0x20`    | User assignable                                               |
| `0x40`    | User assignable                                               |
| `0x80`    | User assignable                                               |


For example, if you wanted to send a [batch response](../docs/communication.md#batch-response-message)
message, the flag byte would be `0x03` (via `0x01 | 0x02`).

### Address

The destination address for the message. You can either send a message to a single node
or broadcast to all nodes.

To broadcast a message to all nodes, set the address field to `0`.

The slave node address space is 1 - 255.

### Message Command

This byte specifies the purpose of the message. Outside of the reserved Command
space, this value is determined by the implementors. For example, your application
might use the command byte `0xA5` to ask a slave node for it's sensor value.

#### Reserved Commands

Commands from `0xFA` to `0xFF` are reserved as commands that should have consistent
meaning across all implementations. Using these for other reasons will likely cause
unexpected problems.

| Command   | Purpose                                   |
|-----------|-------------------------------------------|
| `0xFA`    | Reset node's address and daisy lines.     |
| `0xFB`    | Automatic addressing message.             |
| `0xFC`    | Reserved, but unused.                     |
| `0xFD`    | Reserved, but unused.                     |
| `0xFE`    | Reserved, but unused.                     |
| `0xFF`    | Null message.                             |

### Lengths

For each message there are 2 length bytes: "_num data sections_" and "_length per section_".
When multiplied together, the result should be the full data section length. The value of
these bytes depends on the type of message:

#### Standard message

For standard messages, the first length byte (_num data sections_) will always be 1
and the second byte (_length per section_) will be the total length of the data section.

#### Batch message

A batch message will either contain, or receive, a data section for every node on the
bus. So the first length byte (_num data sections_) should be the number of nodes on the
bus and the second byte (_length per section_) will be the length of data per node.

For example, if we have 5 nodes on the bus, and want to send 3 bytes to each (i.e. RBG color values)
in a batch message, the length values would be: `0x05 0x03` and the full data section size should be
15 bytes long.

## Data Section

The data section is the body of the message and contains all the data you want to send to, or
receive from, the slave nodes.

## Message Footer

The final two bytes of the message make up the 16-bit CRC checksum and mark the end of the message.

The CRC should initially be set to `0xFFFF`, and all message bytes, after the starting two (`0xFF 0xFF`)
will be added to it based on the CRC-16-ANSI algorithm.

A C implementation of the CRC-16-ANSI algorithm can be found [here](http://www.nongnu.org/avr-libc/user-manual/group__util__crc.html#ga95371c87f25b0a2497d9cba13190847f)
