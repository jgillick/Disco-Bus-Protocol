# Disco Bus Message Examples

## A Simple Hello World message {#hello-world}

This is a standard message which sends the bytes: `hi` to node 1.

<table>
  <thead>
    <tr>
      <th colspan="7">Header</th>
      <th colspan="2">Data</th>
      <th colspan="2">Footer</th>
    </tr>
    <tr>
      <th colspan="2">Start</th>
      <th>Flags</th>
      <th>Addr</th>
      <th>Cmd</th>
      <th colspan="2">Lengths</th>
      <th colspan="2">Data</th>
      <th colspan="2">CRC</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0xFF</td>
      <td>0xFF</td>
      <td>0x00</td>
      <td>0x01</td>
      <td>0xA1</td>
      <td>0x01</td>
      <td>0x02</td>
      <td>'h' (0x68)</td>
      <td>'i' (0x69)</td>
      <td>0xDA</td>
      <td>0xE8</td>
    </tr>
  </tbody>
</table>

## Simple Response Message

This asks node 5 which buttons have been pressed (fake command `0x21`) and expects a 4 bytes in response.

The "Who" row shows which device is writing to the bus.

<table>
  <thead>
    <tr>
      <th></th>
      <th colspan="7">Header</th>
      <th colspan="4">Data</th>
      <th colspan="2">Footer</th>
    </tr>
    <tr>
      <th></th>
      <th colspan="2">Start</th>
      <th>Flags</th>
      <th>Addr</th>
      <th>Cmd</th>
      <th colspan="2">Lengths</th>
      <th colspan="4">Data</th>
      <th colspan="2">CRC</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td></td>
      <td>0xFF</td>
      <td>0xFF</td>
      <td>0x02</td>
      <td>0x05</td>
      <td>0x21</td>
      <td>0x01</td>
      <td>0x04</td>
      <td>0x01</td>
      <td>0x01</td>
      <td>0x00</td>
      <td>0x01</td>
      <td>0x40</td>
      <td>0x83</td>
    </tr>
    <tr>
      <th>Who</th>
      <td colspan="7">Master</td>
      <td colspan="4">Node 5</td>
      <td colspan="2">Master</td>
    </tr>
  </tbody>
</table>

## Broadcast Hello World Message

This message sends 'hi' to all the nodes on the bus. This is the same as the [first example](#hello-world), 
except the address is set to broadcast: `0x00`.

<table>
  <thead>
    <tr>
      <th colspan="7">Header</th>
      <th colspan="2">Data</th>
      <th colspan="2">Footer</th>
    </tr>
    <tr>
      <th colspan="2">Start</th>
      <th>Flags</th>
      <th>Addr</th>
      <th>Cmd</th>
      <th colspan="2">Lengths</th>
      <th colspan="2">Data</th>
      <th colspan="2">CRC</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0xFF</td>
      <td>0xFF</td>
      <td>0x00</td>
      <td>0x00</td>
      <td>0xA1</td>
      <td>0x01</td>
      <td>0x02</td>
      <td>'h' (0x68)</td>
      <td>'i' (0x69)</td>
      <td>0xDA</td>
      <td>0xE8</td>
    </tr>
  </tbody>
</table>

## Batch Send Message

Sends RGB color values to 2 nodes. This is a broadcast message (address `0x00`) with flags
set to batch mode message (`0x01`). Data lengths are set as: 2 nodes, with 3 bytes of data each

The "For" row shows which device that part of the message is for.

<table>
  <thead>
    <tr>
      <th></th>
      <th colspan="7">Header</th>
      <th colspan="9">Data</th>
      <th colspan="2">Footer</th>
    </tr>
    <tr>
      <th></th>
      <th colspan="2">Start</th>
      <th>Flags</th>
      <th>Addr</th>
      <th>Cmd</th>
      <th colspan="2">Lengths</th>
      <th colspan="9">Data</th>
      <th colspan="2">CRC</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td> </td>
      <td>0xFF</td>
      <td>0xFF</td>
      <td>0x01</td>
      <td>0x00</td>
      <td>0xC0</td>
      <td>0x02</td>
      <td>0x03</td>
      <td>0xFF</td>
      <td>0x85</td>
      <td>0x90</td>
      <td>0x00</td>
      <td>0xDD</td>
      <td>0x00</td>
      <td>0x80</td>
      <td>0x60</td>
      <td>0xB3</td>
      <td>0xDB</td>
      <td>0x61</td>
    </tr>
    <tr>
      <th>For</th>
      <td colspan="7">All</td>
      <td colspan="3">Node 1</td>
      <td colspan="3">Node 2</td>
      <td colspan="3">Node 3</td>
      <td colspan="2">All</td>
    </tr>
  </tbody>
</table>

## Batch Response Message

This message requests the button status from all 4 nodes on the bus. This is a broadcast message 
(address `0x00`) with flags set to batch & response message (`0x03` = `0x01` | `0x02`). Data lengths 
are set as: 4 nodes, with 1 byte of data each.

The "Who" row shows which device is writing to the bus.

<table>
  <thead>
    <tr>
      <th></th>
      <th colspan="7">Header</th>
      <th colspan="4">Data</th>
      <th colspan="2">Footer</th>
    </tr>
    <tr>
      <th></th>
      <th colspan="2">Start</th>
      <th>Flags</th>
      <th>Addr</th>
      <th>Cmd</th>
      <th colspan="2">Lengths</th>
      <th colspan="4">Data</th>
      <th colspan="2">CRC</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td> </td>
      <td>0xFF</td>
      <td>0xFF</td>
      <td>0x03</td>
      <td>0x00</td>
      <td>0xBB</td>
      <td>0x04</td>
      <td>0x01</td>
      <td>0x00</td>
      <td>0x00</td>
      <td>0x01</td>
      <td>0x01</td>
      <td>0xEF</td>
      <td>0xE3</td>
    </tr>
    <tr>
      <th>Who</th>
      <td colspan="7">Master</td>
      <td>Node 1</td>
      <td>Node 2</td>
      <td>Node 3</td>
      <td>Node 4</td>
      <td colspan="2">Master</td>
    </tr>
  </tbody>
</table>
