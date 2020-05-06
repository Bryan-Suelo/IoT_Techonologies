# LoRa and LoRaWAN 

**LoRa** is a wireless transceiver from Semtech and a modulation technique, while **LoRaWAN** is a software protocol based on LoRa. You can use LoRa transceivers or modulation in your applications, but using LoRaWAN gives you all the benefits of an industrial standard: a strong alliance – the **LoRa Alliance** – that develops compliant products and services, including certified off the shelf modules and network servers, plus a strong security and network control mechanisms. 

## LoRa
Also called Chrip Spread Spectrum Modulation Technology.
LoRa is utilized in unlicensed spectrum below 1GHz, which come at no cost to the applications that use it.
Physical Layer
Data rate : 300bps - 11kbps

## LoRaWAN characteristics
LoraWAN is a software standard which specifies the messages sent between devices and the network, and the way they are processed by the network.

### Main characteristics
* Long range (2-3 km urban, >5 km suburban, >50 km with visual line of sight)
* Long battery life (>1 years)
* Low data rate (0.3 – 50 kbps)
* Operates in the unlicensed spectrum
* Geolocation support

#### Network components:
* Device
* Gateway
* Network server
* Application server
* Join server

#### Frequency bands:
* below  GHz
* High frequencies - high bandwidth
* Low frequencies - long range communication
* Unlicensed spectrum - free to use
* ISM bands (industrial, scientific, specific)

#### Data rates
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

### LoRaWAN architecture
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

### Operating modes
Classes define when a device can recieve downlink messages. Before deciding on which class to use consider this variables:
* Power consumption
* Acceptable latency for downlink messages
#### Class A
All devices have to be compliant with class A.
* Send at any time
* Recieve directly after an uplink
* Device always initiates communication
* Most battery efficient
#### Class B
* Receive downlink messages at specific time intervals
* consumes more energy than class A
* Beaconing, the network syncs the time interval by sending beacons
#### Class C
* Receives downlink messages at any time
* Requires the antenna to always be powered on
* consumes most energy
* Continuous listening 


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


