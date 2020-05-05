# LoRa and LoRaWAN 

**LoRa** is a wireless transceiver from Semtech and a modulation technique, while **LoRaWAN** is a software protocol based on LoRa. You can use LoRa transceivers or modulation in your applications, but using LoRaWAN gives you all the benefits of an industrial standard: a strong alliance – the **LoRa Alliance** – that develops compliant products and services, including certified off the shelf modules and network servers, plus a strong security and network control mechanisms. 

## LoRa
LoRa is utilized in unlicensed spectrum below 1GHz, which come at no cost to the applications that use it.

### Spectrum, Quality of Service and Cost
LoRaWAN™ uses free unlicensed spectrum and is an asynchronous protocol, which is optimal for battery lifetime and cost. 
LoRa and the LoRaWAN protocol have unique features and were designed to handle interference, overlapping networks, and scalable capacity for very high volumes; however, they cannot offer the same **quality of service** (QoS) as a time slotted cellular protocol. 
The low cost, high volume solutions prefer LoRa.

### Battery lifetime
There are two important aspects to consider for battery lifetime: 
* Both the end-device current consumption (peak and average)
* The contribution of protocol.

LoRaWAN is **ALOHA-based** protocol, which means an enddevice can sleep for as little or as long as the application desires. 

While the modulation used in cellular networks is the most efficient to utilize the spectrum, it is not efficient from an end-device perspective. Cellular modulation (OFDM or FDMA) requires a linear transmitter to create the modulation, and a linear transmitter requires orders of magnitude more peak current than non-linear modulations, such as LoRa. These higher peak currents drain the battery faster and require expensive batteries to support them.

## LoRa Alliance
An open, non-profit association of members that believe
the IoT era is now. The Alliance’s mission is to standardize LPWA networks beingdeployed around the world to enable IoT, M2M, and industrial and consumer applications. 
The Alliance members collaborate to drive the global success of the
LoRaWAN™, by sharing knowledge and experience to guarantee interoperability between operators in one open global standard.

A first and crucial step for the Alliance has been to define the initial release of **LoRaWAN™**, which is available on the website for any company who wants to benefit from an open standard.
The Technical Committee of the Alliance is responsible for future enhancements drawing on inputs from all Committee Members to ensure the success of LoRaWAN™ in the IoT.


