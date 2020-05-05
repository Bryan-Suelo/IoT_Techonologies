# IoT_Techonologies

## Internet of Things
### Introduction to the Internet of Things
The term Internet of Things (often abbreviated IoT) was coined by **Kevin Ashton** in 1999 and has emerged into mainstream public view only recently. 

>The **Internet of Things** represents a general concept for the ability of networked objects that collect data from the world around us and share that data across the Internet where it can be processed and utilized for various interesting purposes.

### Four factors of IoT
1. Low cost and High Speed connectivity.
2. Adoption of IP-based networking
3. Computing Economics
4. Advances in data science

### Communication models
* **Device** to **Device**
Two or mas devices are communicated with each other. 
E.g.: Bluetooth, Z-Wave or Zigbee.
* **Device** to **Cloud**
The device is connected directly to an internet cloud service to exchange data and control data traffic.
Use traditional wired Ethernet or Wi-Fi to stablish a connection between the device and the IP network.
E.g.: Smart therostat, sends data to the cloud and data is analized.
* **Device** to **Gateway**
The device connects through an intermediate service to reach a cloud service.
A gateways acts as an intermediary between the device and the cloud service.

## NB-IoT
NB-IoT and cellular communication use licensed bands that are also less than 1GHz. The sub-GHz frequency bands between 500MHz and 1GHz
are optimal for long range communication and the physical size and efficiency of antennas.

### Spectrum, Quality of Service and Cost
Licensed band spectrum auctions of the sub-GHz spectrum are typically greater than 500 million dollars per MHz. While the cellular and NB-IoT time slotted synchronous protocol is optimal from a QoS perspective.
Due to the QoS and the high spectrum cost, higher value applications that need guaranteed QoS prefer the cellular options.

### Battery lifetime
Cellular communication systems are designed for optimal spectrum utilization, which compromises the end-node in terms of cost and battery lifetime. 

In a cellular-based synchronous protocol, the end-device must check-in with the network periodically.
For example, an average cell phone today has to synchronize with the network every 1.5 seconds even while out of use. In NB-IoT, synchronization happens less often but still regularly, which still consumes additional energy from the battery.

While the modulation used in cellular networks is the most efficient to utilize the spectrum, it is not efficient from an end-device perspective. Cellular modulation (OFDM or FDMA) requires a linear transmitter to create the modulation, and a linear transmitter requires orders of magnitude more peak current than non-linear modulations, such as LoRa. These higher peak currents drain the battery faster and require expensive batteries to support them.