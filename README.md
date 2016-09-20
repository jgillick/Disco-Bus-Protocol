# Disco Bus Multidrop Communication Protocol

A versatile master/slave protocol well suited for multidrop networks, like RS485. 

# Features

 * Can automatically assign addresses to all slave nodes.
 * Supports up to 254 slave nodes.
 * Resilient to dead nodes. (i.e. a dead slave node will not kill the rest of the bus)
 * Can send up to 255 bytes of data per node per message.

# Docs

 * [Overview](docs/overview.md)
 * [Communication Basics](docs/communication.md)
 * [Messages](docs/messages.md)
 * [Addressing](docs/addressing.md)
 * [Examples](docs/examples.md)

# Implementations
 * [AVR](https://github.com/jgillick/AVR-Libs/tree/master/MultidropBusProtocol/)