[:arrow_up: Index](#README.md)

# Automatic Addressing

To enable automatic addressing, your bus needs to include an extra daisy chain
line between all nodes on the bus.

![RS485 with Daisy](../images/rs485-daisy.png?raw=true)

The daisy chain lines should maintain an active `LOW`. This means that, at idle, the
line should be pulled up to `VCC`. In order to make a daisy line active, drop it to 
`GND`.

## How it works

 1. Master will start by broadcasting a `reset` message (command: `0xFA`) to all nodes. This should unset their addresses.
 2. Master sends the header for an address batch response message (command: `0xFB`, lengths `0x00 0x02`).
    * **Special case**: the first length byte is `0x00` because we don't know how many nodes there are yet. The address message is the only case where we can send a batch response message with no known slave nodes. 
 3. Master enables it's outgoing daisy line to the first node.
 4. Master sends `0x00` as the first data byte of the message.
 5. The first node sees it's daisy line is enabled and increment the received byte by 1 to `0x01` and sends that to the bus.
 6. Master receives `0x01` and confirms this address by repeating it back to the bus.
 7. The first node receives the confirmation, assigns address `0x01` to itself and enables it's outgoing daisy line to the second node.
 8. The second node sees it's daisy line is enabled and increment the last received address by 1, to `0x02` and sends that to the bus.
 9. Repeat steps 6 - 8 until we have addressed all nodes.
 10. When no new addressed have been received by a timeout period (determined by implementation), master sends `0xFF 0xFF` to complete the message. (No CRC is used to end an addressing message)

To visualize it a different way, here's what happens in the data section of the address message:

| Device   | Byte      | Action / _Note_                         | 
|----------|-----------|-----------------------------------------|
| Master   |           | Sends address message header            |
| Master   |           | Enables daisy line                      |
| Master   | 0x00      |                                         |
| 1st Node | 0x01      | _`0x00 + 1 = 0x01`_                     |
| Master   | 0x01      |                                         |
| 1st Node |           | Enables outgoing daisy line             |
| 2nd Node | 0x02      | _`0x01 + 1 = 0x02`_                     |
| Master   | 0x02      |                                         |
| 2nd Node |           | Enables outgoing daisy line             | 
| ...      |           | _...continue until no new addresses..._ |
| Master   | 0xFF 0xFF | _End message_                           |

## Handling Errors

If a node sends an invalid address back to master (one that is not 1 greater than the last verified address),
master will send `0x00` to the line, to indicate that was an invalid address, and then sends the last
verified address again.

Using the previous example, the exchange between nodes would look like this if the 2nd node
send an invalid address:

| Device   | Byte      | Action / _Note_                         | 
|----------|-----------|-----------------------------------------|
| Master   |           | Sends address message header            |
| Master   |           | Enables daisy line                      |
| Master   | 0x00      |                                         |
| 1st Node | 0x01      | _`0x00 + 1 = 0x01`_                     |
| Master   | 0x01      |                                         |
| 1st Node |           | Enables outgoing daisy line             |
| 2nd Node | 0x05      | **_Send invalid address_**              |
| Master   | 0x00      | **_ERROR_**                             |
| Master   | 0x01      | **_Resend last valid address_**         |
| 2nd Node | 0x02      | **_Send valid address_**                |
| Master   | 0x02      | **_Confirmed address_**                 |
| 2nd Node |           | Enables outgoing daisy line             | 
| ...      |           | _...continue until no new addresses..._ |
| Master   | 0xFF 0xFF | _End message_                           |

## A Different Ending

As stated above, the default way to complete addressing is to wait a predetermined amount of time
for no new addresses. There are a couple ways we can do away with the timeout and make this more 
efficient.

### Incoming Daisy to Master

If we take the outgoing daisy line from the last slave node and send it to an input on master, 
we will be informed as soon as the last node has been given an address.

### Signal Line

If you're bus has a [signal line](../docs/overview.md#signal-line), we can use that to determine when
addressing is complete. 

Here's how that works:

 1. All slave nodes with unset addresses will enable the signal line when they see the start of the address message.
 2. As each node is assigned an address, it will disable it's hold on the signal line.
 3. When the last node disables it's hold on the signal line, the entire line will be disabled and master can end the addressing message.

## Continuing Where You Left Off 

If addressing is interrupted, or you want to run it where you left off (just in case there are any
stragglers) you can easily do that. In this case, do _NOT_ send the `reset` command (`0xFA`) before 
the address message and the first data byte should be the last known address in your bus.
