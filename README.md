# Disco Bus Multidrop Communication Protocol

A versatile master/slave communication protocol originally intended
for modular multidrop networks, like RS485, and providing the ability
for automatic addressing of all nodes.

# Features

 * Can automatically assign addresses to all slave nodes, by order in the bus.
 * Supports up to 254 slave nodes.
 * Can send up to 255 bytes of data per node per message.
 * Resilient to dead slave nodes. (i.e. a dead slave node will not kill the rest of the bus)

# Docs

 * [Overview](docs/overview.md)
 * [Communication Basics](docs/communication.md)
 * [Messages](docs/messages.md)
 * [Addressing](docs/addressing.md)
 * [Examples](docs/examples.md)

# Implementations
 * [AVR](https://github.com/jgillick/AVR-Libs/tree/master/MultidropBusProtocol/)