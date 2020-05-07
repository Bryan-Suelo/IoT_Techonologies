# LoRa and LoRaWAN 

**LoRa** is a wireless transceiver from Semtech and a modulation technique, while **LoRaWAN** is a software protocol based on LoRa. You can use LoRa transceivers or modulation in your applications, but using LoRaWAN gives you all the benefits of an industrial standard: a strong alliance – the **LoRa Alliance** – that develops compliant products and services, including certified off the shelf modules and network servers, plus a strong security and network control mechanisms. 

## LoRa
Also called Chrip Spread Spectrum Modulation Technology.
LoRa is utilized in unlicensed spectrum below 1GHz, which come at no cost to the applications that use it.
Physical Layer
Data rate : 300bps - 11kbps

## LoRaWAN
### LoRaWAN characteristics
LoraWAN is a software standard which specifies the messages sent between devices and the network, and the way they are processed by the network.

#### Main characteristics
* Long range (2-3 km urban, >5 km suburban, >50 km with visual line of sight)
* Long battery life (>1 years)
* Low data rate (0.3 – 50 kbps)
* Operates in the unlicensed spectrum
* Geolocation support

##### Network components:
* Device
* Gateway
* Network server
* Application server
* Join server

##### Frequency bands:
* below  GHz
* High frequencies - high bandwidth
* Low frequencies - long range communication
* Unlicensed spectrum - free to use
* ISM bands (industrial, scientific, specific)

##### Data rates
1. LoRa Modulation 0.3 - 11kbps
2. FSK modulation 50 kbps

**High data rates**
* Small spreading factor
* High probability of packet loss

**Small data rates**
* High spread factor
* Longer time on air
* More power consumption
* Longer range
* Reduces gateway capacity

#### LoRaWAN architecture
Primarily consists of 4 parts:
* End Devices (also referred as Nodes)
* Gateway (Base stations)
* Network Server
* Application Server
#### End devices
* Send and receive data
* Sensing capability
* Computing power
* Last on batteries for months or years
* Bi-directional communication
* 3 operating modes: class A, class B and class C
#### Gateways
* Also called hostpot or access point
* Pick up LoRaWAN packets that are sent by the end device
* Send data back to the end devices
* Demodulate the received signals to a binary buffer
* Backhault: Ethernet, WiFi, Cellular
* Metadata:
    * Received Signal Strength Indicator (RSSI)
    * Signal to Noise Ratio (SNR)
    * Time of Arrival (ToA)
    * Channel
* Sends all received data packets to the Network Server
* Network server:
    * Public or Private
    * Hosted in the cloud, on premises, on the gateway itself
* Gateway are usually transparent
* All LoRaWAn packets received by the gateways are sent to the network, including the ones that are not associated with the network.
* Gateways don't know the devices and send all packages to the network server
* Handle thousands of devices
* Listen on multiple channels simultaneously
* 8, 16 or 64 channels
* Align with the Regional Parameters of the LoRa Alliance
#### Network Server
Key features:
* Connection with gateways
* Commissioning and supervision interface
* Deduplication of packets
* Device registry with:
    * Security sessions
    * Operation modes
    * Timing windows
    * Frequencies
    * Data rate for downlink
* Device identification with help of the session integrity key
* Control data rate through ADR (Adaptative Data Rate)
* Set radio parameters and timing windows
* Media Access Control (MAC) commands
* Prevent replay attacks through frame counters
    
All gateways forward the messages to a Network Server. This Network Server is where more complicated process and data handling takes place. It is responsible for:
* Routing / forwarding messages to the Application Server
* Selecting the best gateway for downlink messages based on Link Quality Indication:
    * RSSI
    * SNR
* Removing duplicate messages if they’re received by multiple gateways
* Encrypting and Decrypting messages sent to and from end devices
#### Application server
* Connected to a network server
* Receives uplink messages from the network server
* Sends messages to the network server
* Recommended to run in the user's domain
* Web interface
* Integrate with IoT platforms 
#### Join server
* Activation of devices
* Session Security keys
    * Network Session key - Network server
    * Application Session key - Application server
* Specificies which Network Server the user wants to work with
* Operated by a trusted third party

### Operating modes
Classes define when a device can recieve downlink messages. Before deciding on which class to use consider this variables:
* Power consumption
* Acceptable latency for downlink messages
#### Class A
All LoRaWAN devices have to support class A.
* Offers the lowest energy option
* Send messages at any time
* A downlink message can only be received directly after sending an uplink message.
* Device always initiates communication
* End devices allow for bidirectional communication.

##### How it works?
Every uplink transmission is followed by 2 short downlink receive windows to allow packets from the server to be received. The transmission slot scheduled by the end node is based on its own communication needs, with a small variation based on a random time basis. This process happens closely after the end node has sent an uplink transmission. Downlink message from the server will have to wait until the next scheduled uplink.

_Uplink example_
![](\Images\Class_A_uplink.png)

The duration of a receive window must be at least the time required by the radio receiver to effectively detect a downlink packet. If the downlink packet is detected during one of the receive windows, the radio receiver remains active until the downlink frame is demodulated. 

_Example Downlink_ 
![](\Images\Class_A_downlink.png)

If a message arrives, the end device doesn't know for whom the packet is meant. First, the packet needs to be demodulated and the device address and Message Integrity Code (MIC), need to be checked. If the packet is intended for the device that receives it, it does not open the second receive window (called Rx2). 
If the device didn't receive the downlink message in the first receive window (referred to as Rx1), then it will open Rx2. The end node doesn't open the second window if the frame intended for a particular end node has correctly been verified during the first window.

_Example transmition_
![](\Images\Class_A_transmit.png)

#### Class B
* End devices using Class B allow for more receive slots
* Devices have extra Rx at scheduled times
* Receive a time synchronized beacon from the gateway. This allows the server to know when the end node is listening
* Receive downlink messages at specific time intervals
* Consumes more energy than class A
* Beaconing, the network syncs the time interval by sending beacons

##### How it works?
When shifting from class A to class B, the device must first receive a network beacon and align its internal timing with the network server. Based on this, the end node can open receive windows or ping slots, which can be used by the network to initiate downlink communication.
A network-initiated downlink message is called a ping. The gateway selected to initiate this downlink is selected by the network server based on signal strengths and quality of uplink messages.

_Example transmition_
![](\Images\Class_B_transmit.png)

Once a device is shifted to Class B operation, it must periodically send a beacon message to cancel any drift that may occur with its internal time clock.

#### Class C
* Receives downlink messages at any time
* Requires the antenna to always be powered on
* Consumes most energy
* Continuous listening 

##### How it works?
Class C devices implement the same 2 receive windows as Class A devices, but they **do not close** the Rx2 until they need to send again.

_Example transmition_
![](\Images\Class_C_transmit.png)

This way they can receive the downlink in the Rx2 window at any time. There is no specific message sequence for the node to tell the server whether it is Class A or C. It is up to the application server to be aware that it manages Class C devices based on the characteristics during the join process

### LoRAWAN networks
#### Public netowrks
In most countries, one or more public LoRaWAN networks are provided by telecom operators. These operators have deployed a number of gateways and allow devices to be registered on their networks once a fee is paid. 
The pricing often depends on the number of connected devices and the number of packets those devices send per day.

As an alternative to these fee-based public networks, there are also networks, such as **The Things Network**, which are public, but not fee-based. The Things Network runs an open, crowdsourced LoRaWAN network. The network is maintained by thousands of people and organizations worldwide, all of which run one or more gateways to ensure network coverage. Everyone is free to add gateways and connect end devices to The Things Network.

#### Public networks
These networks can be created and maintained by individuals or companies. There are also companies that specialize in deploying and managing private networks.

#### Choosing a Network Provider
Things to consider when choosing a network server include:
* Pricing
* Gateway access control
* Privacy

When only connecting a handful of devices, using a public network provider is probably the easiest and most cost-effective option.
When deploying a large number of devices (or in a case where all devices are in a dense environment like a building, farm or university) it can be more cost effective to operate the gateways yourself.

For compliance reasons, some companies have strict data privacy and governance rules, which may require that the company only use its own servers for routing and storing the data collected by its end devices.

##### When to Use a Public Network
Using a public LoRaWAN network is easy – there is no need to install your own gateways; generally all you need to do is purchase a subscription and you can then connect your devices to the network. When using a public network, make sure to verify the network coverage before committing to a network subscription. This is important because it is not possible to optimize network coverage by adding additional gateways.

Also, be aware of the fact that if you are using a public infrastructure, the network operator will manage the security keys of your devices.

##### When to Use a Private Network
Companies with strict data governance and security rules may choose to opt-in for a private LoRaWAN network. This allows the company to manage the security of its data directly. By running the Application Server and Join Server yourself, or by engaging a trusted third party, the safety of the keys can be assured. 
Also, when rolling out a large number of end devices, it can be more cost effective to run a private LoRaWAN network.

The downside of private networks, however, is the associated overhead: gateways need to be installed and maintained. Additionally, the network server needs to be managed. Fortunately, there are third-party companies that specialize in this work.

[LPWA Technologies - Unlock New IoT Market Potential](https://lora-developers.semtech.com/uploads/static/LPWAN_technologies.pdf)
### Spectrum, Quality of Service and Cost
LoRaWAN™ uses free unlicensed spectrum and is an asynchronous protocol, which is optimal for battery lifetime and cost. 
LoRa and the LoRaWAN protocol have unique features and were designed to handle interference, overlapping networks, and scalable capacity for very high volumes; however, they cannot offer the same **quality of service** (QoS) as a time slotted cellular protocol. 
The low cost, high volume solutions prefer LoRa.

### Battery lifetime
There are two important aspects to consider for battery lifetime: 
* Both the end-device current consumption (peak and average)
* The contribution of protocol.

LoRaWAN is **ALOHA-based** protocol, which means an enddevice can sleep for as little or as long as the application desires. 
LoRaWAN supports class B, which was designed to reduce the downlink communication latency by having the end-device wake up at programmable intervals to check for downlink messages.

For applications that need very long battery lifetime and optimized cost, but do not need to communicate as often, LoRa is a better option.

### Network coverage
LoRa components and the LoRaWAN ecosystem are mature and production-ready
now, while nationwide deployments are still in the rollout phase. One significant attribute of the LoRaWAN ecosystem is the components’ operability in the private or enterprise model as well as the public network model. Many large enterprises are planning a hybrid model that deploys a network in their facility and utilizes the public network for coverage outside of their facility.

### Device cost
There are no royalty issues to create margin stacking in the LoRa Alliance community so modules under four dollars are much more feasible in the LoRaWAN ecosystem. Certified LoRaWAN modules today are in the $7-10 range but are expected to reach the $4-5 as integration and volumes mature within the ecosystem. Alternatively, today cellular LTE modules struggle to
get below $20.
For IoT and LPWAN, different deployment models will be used to lower the CapEx and OpEx costs of deployment compared to traditional deployment models solely based on towers. LoRaWAN deployments will cost much less, utilizing a mix of traditional towers, industrial gateways and in-home pico gateways. The price of tower top gateway is in the range of $1000, industrial gateways are less than $500 and low cost pico cell gateways are in the range of $100.

## LoRa Alliance
An open, non-profit association of members that believe
the IoT era is now. The Alliance’s mission is to standardize LPWA networks beingdeployed around the world to enable IoT, M2M, and industrial and consumer applications. 
The Alliance members collaborate to drive the global success of the
LoRaWAN™, by sharing knowledge and experience to guarantee interoperability between operators in one open global standard.

A first and crucial step for the Alliance has been to define the initial release of **LoRaWAN™**, which is available on the website for any company who wants to benefit from an open standard.
The Technical Committee of the Alliance is responsible for future enhancements drawing on inputs from all Committee Members to ensure the success of LoRaWAN™ in the IoT.


