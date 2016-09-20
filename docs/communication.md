# Disco Bus Communication Basics

The master device manages all messages on the bus. Slave nodes respond to requests from
master by filling in their data section of a message.

All messages are directed either at a single node or all nodes (i.e. broadcast).

# Types of Messages

## Standard message

A standard message is one that is sent from master to one or all nodes on the bus.
The entire data section is intended for the recipient(s).

## Batch message

A batch message is broadcast to all nodes, with a unique data section included for each
node. This works by dividing the entire data section up into equal parts for all nodes
on the network.

For example, if our network contained 3 nodes, and master wanted to send RGB color values to each,
the data section might look like this:

<table style="text-align: center;">
  <thead>
    <tr>
      <th>Msg Header</th>
      <th colspan="9">Message Data</th>
      <th>Msg Footer</th>
    </tr>
    <tr>
      <th></th>
      <th colspan="3" align="center">Slave 1 Data</th>
      <th colspan="3" align="center">Slave 2 Data</th>
      <th colspan="3" align="center">Slave 3 Data</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>...</td>
      <td>0xFF</td>
      <td>0x00</td>
      <td>0x00</td>
      <td>0xd0</td>
      <td>0x33</td>
      <td>0x10</td>
      <td>0x00</td>
      <td>0x00</td>
      <td>0x66</td>
      <td>...</td>
    </tr>
  </tbody>
</table>

## Response Message

If master wants to receive a response from a slave node, it starts a response message,
and waits for the node to fill in the data before closing the message.


### Batch response messages

Batch response messages, work the same as batch messages, but expect a response from all nodes
in the data section of the message.

### Response timeout

If a node does not add the response to a message before the timeout occurs, master
should fill the data section with default values. The timeout length and default values
are implementation specific.

The master device must also handle the case that a node only enters partial data.
