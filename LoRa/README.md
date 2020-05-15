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

#### Private networks
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

### Roaming
#### Introduction to Roaming
Roaming defines the mechanism of exchanging traffic between network servers, and there are a number of cases where roaming is advantageous. Roaming is good for extending network coverage. Indeed, even when a device is within range of gateways connected to its home network server, it may still be beneficial to enable roaming.

Additionally, using a lower spreading factor and a higher data rate increases network capacity. The reason for this is that there is less noise and less channel use in this circumstance, leading to fewer packet collisions.
Another benefit of having more gateways receive LoRaWAN packets is the increased geolocation accuracy. This is due to the fact that more metadata, such as timestamps and signal strength, is received thanks to the additional gateways.

#### LoRaWAN Roaming Architecture
There are three different roles the network server plays when a device is roaming. 

![](\Images\Roaming_architecture.jpg)

##### Home Network Server (hNS)
An end device and its corresponding application are registered with its home network server. This network server is connected to a Join Server where the device's root keys are stored. It is also connected to the application server, which decrypts and encrypts the application data.
##### Serving Network Server (sNS)
The serving network server is used to activate end devices. It controls the MAC layer of the end device to define the RF parameters. It also validates integrity checks.
##### Forwarding Network Server (fNS)
The forwarding network server manages the gateways. It simply forwards and receives traffic to and from end devices.

#### Types of roaming
##### Passive Roaming
In passive roaming, as illustrated below, an end device registered with Operator #1 sends a message via a gateway that is part of the network managed by Operator #2. The message is received by the fNS in Operator #2’s network. Presuming that Operators #1 and #2 have a roaming agreement in place, Operator #2 will pass the data packets from Operator #1’s end device to Operator #1’s sNS.

![](\Images\Passive_roaming.jpg)

When a network server is configured to enable passive roaming, it looks at the first seven bits of the **device address** (DevAddr) which is used as the **network identifier** (NwkID). The NwkID indicates the device’s hNS operator. If a roaming agreement is in place with the operator, the message is passed along. If no match is found, the message is dropped.
##### Handover Roaming
Only applies to devices on networks that follow the **LoRaWAN 1.1 specification**. This specification allows for transferring the MAC-layer control of a device to a different network server provider. This means that the sNS does not have to be operated by the same provider that runs the home network server (which is the case with passive roaming). With handover roaming, a _foreign_ network server can control the RF parameters such as the channel plan and Rx windows.

![](\Images\Handover_roaming.jpg)

### LoRaWAN specification
Defines the standardized set of rules to be followed when using a LoRaWAN network. Introduced in 2015, the specification prescribes:
* The standardized encryption format and the creation of the Message Integrity Code (MIC).
* How a LoRaWAN message should be constructed, both for uplinks and downlinks, as is the case for the joining procedure.
* The available MAC commands.
* Describes the LoRaWAN devices classes (A, B and C), the receive window slots and much more.

#### LoRaWAN 1.1
##### Back-End interfaces
A companion document, **LoRaWAN Back End Interfaces V1.0**, was released with LoRaWAN Specification 1.1. This supplemental document standardizes the functionality and message flow of the various network components and the communications among them. This makes it easier for different network providers to interconnect their networks, which is necessary for successful roaming.

The document introduces a new component called the Join Server. The Join Server stores LoRaWAN root keys. Because interfaces and message flows are standardized, it is possible to let a third party run and manage this server for enhanced security. Because the Join Server runs independently of the Network Server and Application Server, end devices can rejoin different LoRaWAN Network Servers over time.

##### Security Enhancements
The LoRaWAN 1.0x specification requires a single, shared, secret key, the Application Key (AppKey). The AppKey is used to derive two session keys: the AppSKey and the NwkSKey.
New in LoRaWAN 1.1 is the **Network Key** (NwkKey). This is a 128-bit key which is used to derive three session keys:
1. **FNwkSIntKey** (_Forwarding Network Session Integrity Key_)
* Used by an end device to calculate the MIC for all uplink messages
2. **SNwkSIntKey** (_Serving Network Session Encryption Key_)
* Used by an end device to verify the MIC for all downlink messages
3. **NwkSEncKey** (_Network Session Encryption Key_)
* Used for the encryption of MAC commands

Another security enhancement relates to the frame counter. With LoRaWAN 1.1, the frame counter can never be reset during a session. This means, when using Authentication by Personalization (ABP), end devices require persistent memory to make sure the frame counter does not start from 0 after a power cycle.
To avoid replay attacks, two nonces are introduced: 
1. **DevNonce** - 
* Counter which starts at 0 when the device is initially powered on. 
* It initiates a join request. 
* The nonce increments with every new join request the device sends.
2. **JoinNonce**. 
* Counter which also starts at 0. 
* It increments every time a join request is accepted (when a join accept is sent by the Join Server). 
* This mechanism prevents replay attacks whereby an attacker sends previously recorded join request messages with the intention of disconnecting the respective end device from the network.
##### Join Procedure
This component stores both of the root keys (NwkKey and AppKey). It also derives the session keys, which are sent to the corresponding Network Server and Application Server.
Because the interface and message flow of all Network Server components is standardized, trusted third parties may operate Join Servers for the secure storage of keys.
##### Roaming
With LoRaWAN 1.1 devices can be controlled by a foreign network. When a roaming agreement is in place, a foreign Network Server can connect to the Join Server of the end device and take full control over the device.

### Security
LoRaWAN utilizes two layers of security: one for the network and one for the application.

**Network layer** is responsible for:
* Identification of the device
* Integrity check
* Network session integrity key
* Network server encrypts MAC commands

These commands change the data rate, enable or disable channels, change downlink frequencies, etc.

**Application Layer** used for:
* Encryption and decryption of application payload
* Application session key
* keys are 128 bit AES keys

#### Device activation
**ABP** - Activation by Personalisation
* Session keys and the device address is programmed in the device

**OTAA** - Over The Air Activation
* More complicate but securest

### LoRa Alliance
An open, non-profit association of members that believe
the IoT era is now. The Alliance’s mission is to standardize LPWA networks beingdeployed around the world to enable IoT, M2M, and industrial and consumer applications. 
The Alliance members collaborate to drive the global success of the
LoRaWAN™, by sharing knowledge and experience to guarantee interoperability between operators in one open global standard.

A first and crucial step for the Alliance has been to define the initial release of **LoRaWAN™**, which is available on the website for any company who wants to benefit from an open standard.
The Technical Committee of the Alliance is responsible for future enhancements drawing on inputs from all Committee Members to ensure the success of LoRaWAN™ in the IoT.

[LPWA Technologies - Unlock New IoT Market Potential](https://lora-developers.semtech.com/uploads/static/LPWAN_technologies.pdf)
#### Spectrum, Quality of Service and Cost
LoRaWAN™ uses free unlicensed spectrum and is an asynchronous protocol, which is optimal for battery lifetime and cost. 
LoRa and the LoRaWAN protocol have unique features and were designed to handle interference, overlapping networks, and scalable capacity for very high volumes; however, they cannot offer the same **quality of service** (QoS) as a time slotted cellular protocol. 
The low cost, high volume solutions prefer LoRa.

#### Battery lifetime
There are two important aspects to consider for battery lifetime: 
* Both the end-device current consumption (peak and average)
* The contribution of protocol.

LoRaWAN is **ALOHA-based** protocol, which means an enddevice can sleep for as little or as long as the application desires. 
LoRaWAN supports class B, which was designed to reduce the downlink communication latency by having the end-device wake up at programmable intervals to check for downlink messages.

For applications that need very long battery lifetime and optimized cost, but do not need to communicate as often, LoRa is a better option.

#### Network coverage
LoRa components and the LoRaWAN ecosystem are mature and production-ready
now, while nationwide deployments are still in the rollout phase. One significant attribute of the LoRaWAN ecosystem is the components’ operability in the private or enterprise model as well as the public network model. Many large enterprises are planning a hybrid model that deploys a network in their facility and utilizes the public network for coverage outside of their facility.

#### Device cost
There are no royalty issues to create margin stacking in the LoRa Alliance community so modules under four dollars are much more feasible in the LoRaWAN ecosystem. Certified LoRaWAN modules today are in the $7 to 10 range but are expected to reach the $4 to 5 as integration and volumes mature within the ecosystem. Alternatively, today cellular LTE modules struggle to get below $20.
For IoT and LPWAN, different deployment models will be used to lower the CapEx and OpEx costs of deployment compared to traditional deployment models solely based on towers. LoRaWAN deployments will cost much less, utilizing a mix of traditional towers, industrial gateways and in-home pico gateways. The price of tower top gateway is in the range of $1000, industrial gateways are less than $500 and low cost pico cell gateways are in the range of $100.

## Hardware
### Hardware components
1. Micro-controller unit
* System on Chip
* Manages device funcionalities
* CPU and memory
* Embedded applications of low-power devices
* Low cost and small in size
2. Power supply
* Provided trough battery
* solar panel
3. Peripherals
* Sensors / LCD screen / Relay switch / etc.
* Communication protocols
    * GPIO
    * SPI
    * I2C
    * UART
### Device software layer
1. Application layer
* Funcional application
2. Middleware layer
* communication protocol libraries - LoRaWAN stacks
* Complex drivers
3. Driver layer    
* Manages peripherals
### End devices
#### (Low) Power Consumption of End Devices
The key to the low power performance is to put a device into this deep sleep mode for over 99,9% of the time. This is the only way to make a small sized-battery last for one or multiple years.
##### Wake up!
Once a device is in deep sleep it can be woken up in two different ways. After a set time interval, or through an interrupt. If a sensor reading needs to be conducted periodically, the device can be programmed to wake up at a specific time intervals. Some sensors (not all) have the ability to wake up a device when it senses something. This is what we call an interrupt.
##### Factors to take into consideration
A device in deep sleep mode can last for years on a battery. The difference in energy consumption of a device in deep sleep mode, compared to the consumption of a device which sends data is about 3 orders of magnitude.
* _Device is awake_ - The time a device is awake and senses its environment with sensors.
* _Sending data_ - Sending data consumes a lot of energy. It can easily consume 1000 times more power compared to a device in deep sleep mode. Try to limit the number of messages sent and use the lowest Spreading Factor (SF) possible.
* _Active radio_ - After a transmission, the LoRaWAN module listens for any incoming messages. This is done twice for a period of 1 second.
* _Receiving data_ - Receiving data from gateways costs a lot of energy as well, but less than sending data. The transmission power of a Microchip module is 40mA, receiving data **only** consumes 14,2mA.

##### Calculating battery life
Batteries have a certain amount of energy they can store, indicated as MilliAmp Hour, abbreviated as mAh. The more mAh a battery contains, the longer a battery can last, this is (most of the times) correlated with the size of the battery itself. If the power a device consumes is known, as well as the capacity of the battery, the battery lifetime can be calculated using the simple formula:

> Battery life (hours) = Battery Capacity (mAh) / Average current drain (mA)
                        
##### Antenna design
