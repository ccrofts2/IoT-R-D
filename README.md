# IoT-R-D

## Introduction
The rapid growth of the Internet of Things (IoT) presents significant security risks due to inherent vulnerabilities in many commercial devices. This report details my research and experimentation focused on developing a secure communication protocol for ESP32 microcontrollers. This project spanned networking and security concepts, from packet analysis to cryptographic implementations. The goal was to address the security deficits commonly found in real-world IoT devices by implementing cryptographic principles, secure key management, and robust authentication mechanisms.

## Research
In my research, I quickly realized that the security of many real-world IoT devices is often shockingly inadequate. I was surprised by the ubiquity of unencrypted communication, the use of default credentials, and the absence of even basic authentication mechanisms. It became clear that a strong understanding of network fundamentals and cybersecurity best practices would be essential to the success of this project.
To get a better grasp on the security problems, I began to study networking. My studies into network architecture led me to the OSI model, where I discovered the complexities of data transmission at each layer. The different layers all serve a purpose and can all be sources of vulnerabilities. I started to learn about the different protocols at each layer, and how they work, and also how they can be exploited. The use of the low-level networking tool, Netcat, became crucial for me to gain an understanding of how the different protocols work. I used Netcat extensively to manually send and receive data, and gained a better understanding of the workings of client and server communication. Through these manual network experiments, I was able to visualize the flow of data, the structure of packets, and the underlying handshakes that make network communication possible. The use of Netcat also gave me a better understanding of some of the vulnerabilities that were present in these systems.
Further study was conducted using the network protocol analyzer, Wireshark. Wireshark became instrumental in my learning process, offering a real-time view of network traffic, where I could examine packets, headers, and payloads in detail. Before Wireshark, I was only looking at the documentation of protocols, or seeing what they were trying to do, but after using Wireshark, I could see the raw output of these protocols, and it furthered my understanding. I focused on how TCP worked, but also explored other protocol options as well. This hands-on experience allowed me to understand not just the theoretical aspects of networking protocols, but also their practical behavior in live network environments. Wireshark's filtering capabilities were particularly helpful for isolating and examining specific traffic, enabling me to investigate particular vulnerabilities in specific protocols. Seeing the data transmitted in plain text in some protocols was eye-opening and reinforced the importance of strong encryption. I learned to identify the different layers of network protocols (such as the Ethernet, IP, TCP/UDP, and application layers), and I also gained experience tracing and following network streams. The level of visibility that Wireshark provided helped me better understand where my vulnerabilities might lay in my code.

## Experimentation
To explore how different protocols work I started building client-server simulations. I created simulations for TLS and MQTT protocols. During this time, I learned about handshakes, data exchanges, and the need for secure key management. I found that TLS had many steps that needed to be taken in order to properly implement the protocol. I also found similar complexities with MQTT. I had to learn about the management of certificates, the different types of brokers, and the various security measures I could take to ensure communications are secure. Using the different brokers also taught me a great deal about secure implementation of these types of systems, and also highlighted many of the vulnerabilities of these systems. Through these experiments, I gained a deeper understanding of the challenges in securing communication, especially the difficulties of managing and securing certificates. I not only saw how these protocols worked, but I also learned some of the practical implications of their implementation.

## Design
With a better understanding of the security landscape, I turned my attention to designing a secure communication protocol for ESP32 microcontrollers using ESP-NOW, a peer-to-peer protocol. I chose ESP-NOW for its lightweight nature, and its ability to communicate without the need for a Wi-Fi router, which was an interesting concept to me. I wanted to better understand the security challenges of peer-to-peer networks, and ESP-NOW seemed ideal for this. I started by doing research into the ESP-NOW protocol, and its security implications. I used my prior research to begin to identify key design principles:

1.	Confidentiality: Protecting data from unauthorized access through encryption.
2.	Integrity and Authenticity: Verifying that data has not been tampered with and comes from a trusted source, through message authentication.
3.	Authentication: Ensuring that only authorized devices can communicate with each other through MAC filtering and a pre-shared key.
4.	Replay Attack Mitigation: Minimizing the risk of attackers replaying captured packets, through sequence numbers and later with the introduction of nonces.
5.	Secure Key Management: Ensuring the safe storage and handling of secret keys to protect the system from compromises.
   
This design approach helped to prioritize security in the constrained environment of IoT devices, and help me make trade-offs that were required by the limited processing and power resources available.
Implementation
I focused my development on the ESP-NOW protocol, given its focus on peer-to-peer communication, which is useful for many IoT applications.
My implementation included the following key security measures:

*	Encryption: I utilized ESP-NOW's built-in encryption, which encrypts data using a pre-shared key (PMK), ensuring the confidentiality of transmitted messages. I learned that the use of a PMK was better than transmitting data in plain text, but also that key management would be important going forward.
*	Key Storage: I stored the PMK, and other keys such as an authentication key, in the ESP32’s NVS (Non-Volatile Storage). Enabling flash encryption prevented external access to the NVS data. This was a step up from hard coding the keys in code, but it was also not ideal, as it was not clear whether this was sufficiently secure. I also learned that secure key management would require more sophisticated methods to ensure that they remained safe.
*	Message Authentication: I implemented HMAC using the mbedtls library to ensure data integrity and message authenticity. Implementing HMAC involved understanding the underlying principles of cryptographic hashing and how to use HMAC to verify messages.
*	MAC Address Filtering: I implemented a layer of authentication based on the MAC address of the sender, which allowed me to prevent spoofing attacks by rejecting messages from unauthorized devices, ensuring that communications are between trusted devices.
*	Sequence Numbers: I implemented the use of sequence numbers in my system to mitigate replay attacks. This made it so that each message that was transmitted had a sequence number that corresponded with the last transmitted message. However, I found that I was vulnerable to sequence number rollover.

## Conclusion
This project underscored that securing IoT devices requires a rigorous understanding of cryptographic principles, networking protocols, and the practical challenges of implementation. Building a secure system necessitates careful consideration of trade-offs between security, complexity, and performance. I gained a deeper understanding of common vulnerabilities in the IoT space and developed a security-centric approach. I learned that many IoT devices exhibit poor security due to design flaws and oversight. I have gained a better understanding of common vulnerabilities in IoT and developed an approach that prioritizes security.
This project highlighted that security is an iterative and ongoing process, not a one-time task. My implementation of the ESP-NOW protocol serves as a foundation, but I recognize the necessity for further refinement. Future development must prioritize:

*	Further Replay Attack Mitigation: Integrating nonces into the message authentication process to fully address replay attacks.
*	Enhanced Key Management: Exploring sophisticated key management strategies for key storage and handling.
*	Configuration Flexibility: Moving hardcoded variables into separate configuration files to streamline system deployment and configuration.

This project has been a valuable learning experience, highlighting that proactive and continuous security measures are vital. I am amazed by the depth of security that is possible. To me, there seems to be no limit to how secure you can make a system. As an electrical engineer, I am always trying to build new devices that make my life more interesting or easier. I am excited to build more devices capable of wireless communication, knowing that I can implement a few layers of security. 
