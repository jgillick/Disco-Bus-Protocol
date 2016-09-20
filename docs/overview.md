# Disco Bus Overview

_Version 1.0_

The Disco Bus protocol is a master/slave bus originally intended for multidrop networks, like RS485,
it can also be used in direct device-to-device setups. One of it's great strengths is
it's ability to automatically assign addresses to all the slaves in the network based on
their location on the bus. Also, since this is not a daisy chain communication bus, if
a node dies at any time, it does not take the rest of the bus down with it.

All communication is managed by the master, and the slaves either act on the messages or
provide responses.

## Network Topology

### Device-to-Device

A simple device-to-device setup.

![Simple Device-to-Device](../images/d2d.png?raw=true)

### Simple RS485

A simple RS485 network, where automatic addressing is not needed.

![Simple RS485](../images/rs485-simple.png?raw=true)

### Addressable RS485

If you need to automatically address the slave nodes, add an additional daisy line.

![RS485 with Daisy](../images/rs485-daisy.png?raw=true)

The daisy chain lines should maintain an active `LOW` and be pulled up to `VCC` by default. 
`GND`.

### Signal Line

There is an optional signal line that can be used for global signaling and assist with addressing.

![RS485 with Daisy](../images/rs485-signal.png?raw=true)

The signal lines should maintain an active `LOW` and be pulled up to `VCC` by default.

## History

This bus protocol was originally conceived while building an [interactive disco dance floor](hackaday.io/project/4209-interactive-disco-dance-floor)
for Burning Man 2015. I needed a protocol that would offer quick 2-way addressable communication for up 
to 144 nodes and not require manual address remapping if nodes got swapped out or moved.

Initially, simple daisy chained SPI buses looked like really attractive options. However, with 
the abuse the floor was going to get, I didn't want a single dead node to stop the communication 
for all the nodes after it. Also, it needed to keep a clean signal on a data line that could
be up to 100 feet long (i.e. twisted pair and RS485 to the rescue).

With the assistance of my friend [Cameron](github.com/cinderblock/), I cobbled together an early version
of this protocol for Burning Man 2015. After the burn, I spent several months retooling and rewriting the
implementation from scratch for Burning Man 2016.

Now I'm putting it out there for other people who might want to use it. I'd love to get feedback
or just hear about the projects you're using it for.

Enjoy!
