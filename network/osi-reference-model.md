# Protocols

Write about network protocols.

## OSI Reference model

OSI reference consists of 7 layers.

### L1

Physical layer.
This enables us to transmit raw bits.

### L2

Data link layer.
This enables us to submit data frames between two nodes.

### L3

Network layer.
This enables us to do multiple node connection with addressing and routing.

#### IP

Internet Protocol

### L4

Transport layer.
Transmission of data between points in the network.
Usually, port is defined in this layer.

#### TCP

Transmission Control Protocol.
This create reliable channel beteween two points.

#### UDP

User Datagram Protocol.
Just providing ports and a checksum in addition to data.

### L5

Session layer.

I don't what this layer do, 
but, for example, RPC (Remote Procedure Call Protocol) is this layer.

### L6

Presentation layer.
I don't what this layer do, 

### L7

Application layer.

#### HTTP

Hyper Text Transfer Protocol.
The name is by historical reason.

### Question?

Where TLS is located?
Between L4 and L7?

