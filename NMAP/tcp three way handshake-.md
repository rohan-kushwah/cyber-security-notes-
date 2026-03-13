# TCP Three-Way Handshake Process Explained

The **TCP three-way handshake** is the fundamental mechanism used to establish a reliable TCP connection between a client and a server. It ensures both sides are synchronized and ready to communicate.

![[Pasted image 20251007155318.png]]

## Step 1: SYN (Synchronize) Packet

- The client initiates the connection by sending a TCP packet with the **SYN flag set**.
- This packet contains an **Initial Sequence Number (ISN)** chosen by the client, which will be used to track the order of data it sends.
- Purpose: Request to open a connection.

## Step 2: SYN-ACK (Synchronize-Acknowledge) Packet

- The server responds to the client's SYN packet with a TCP packet that has both **SYN and ACK flags set**.
- The **SYN flag** means the server agrees to establish a connection.
- The **ACK acknowledges the client's ISN** by setting the acknowledgment number to `client's ISN + 1`.
- The server also includes its own ISN for the connection.

## Step 3: ACK (Acknowledge) Packet

- The client responds back with a TCP packet with the **ACK flag set**.
- The acknowledgment number is set to **server's ISN + 1**, acknowledging the server's SYN-ACK.
- After sending this packet, the connection is established, and both sides can begin data transmission.

## Why Is the Three-Way Handshake Important?

- **Reliability:** Both client and server confirm they are ready to communicate, synchronize their sequence numbers, and establish connection parameters.
- **Connection Establishment:** Without this handshake, TCP wouldn't reliably ensure both parties are prepared, risking lost or misordered data.
- **Duplicate Data Prevention:** Sequence and acknowledgment numbers help manage data order, ensuring duplicates are detected and avoided.

## Summary Table

| Step | Packet Type | Flags Set | Purpose |
|------|-------------|-----------|---------|
| 1 | SYN | SYN | Client requests connection; sends ISN |
| 2 | SYN-ACK | SYN + ACK | Server acknowledges client, sends its own ISN |
| 3 | ACK | ACK | Client acknowledges server's SYN-ACK |

This handshake underpins TCP's ability to provide a reliable, ordered, connection-oriented communication channel, making it fundamental to network communication and also to many Nmap scanning techniques that analyze or mimic these packet exchanges.

---
----

# in tcp https use different part after handshape called tls handshake -
# TLS 1.2 Handshake Process

The TLS (Transport Layer Security) handshake is a crucial process that establishes a secure, encrypted connection between a client and a server. This handshake involves several steps where both parties agree on protocols, authenticate each other, and generate session keys for encryption. Below is a detailed explanation of the TLS handshake process:

## TLS Handshake Process Steps

1. **Client Hello**
   - The client sends a "Client Hello" message to the server.
   - This message contains the TLS version supported by the client, a list of supported cipher suites (cryptographic algorithms), and a random value called the **Client Random**.

2. **Server Hello**
   - The server responds with a "Server Hello" message.
   - This message includes the TLS version the server selected (from the client's list), the cipher suite chosen, and a random value called the **Server Random**.
   - The server also sends its **digital certificate**, which contains its public key and identity information to authenticate itself.

3. **Server Authentication and Certificate Verification**
   - The client verifies the server's certificate against trusted Certificate Authorities (CAs) to ensure the server is authentic.
   - If the server requires client authentication, it also requests the client's certificate.

4. **Client Key Exchange (Pre-Master Secret)**
   - The client generates a **pre-master secret**, a random number used to create the master secret.
   - The client encrypts this pre-master secret with the server's public key (from the certificate) and sends it to the server.

5. **Server Decrypts Pre-Master Secret**
   - The server uses its private key to decrypt the pre-master secret.
   - Both client and server now have the pre-master secret.

6. **Session Keys Generation**
   - Using the **Client Random**, **Server Random**, and **pre-master secret**, both parties independently compute the same **master secret**.
   - From the master secret, symmetric **session keys** are derived for encrypting and decrypting the data.

7. **Change Cipher Spec**
   - The client sends a "Change Cipher Spec" message, indicating that future messages will be encrypted using the negotiated session key.
	   - The client then sends a "Finished" message encrypted with the session key, proving the handshake's integrity

8. **Server Change Cipher Spec and Finished**
   - The server sends its own "Change Cipher Spec" message and an encrypted "Finished" message.
   - Once the client decrypts and verifies this, the handshake is complete.

9. **Secure Encrypted Communication Begins**
   - Both parties now exchange data encrypted with the symmetric session keys for confidentiality and integrity.

## Summary Table of TLS Handshake Steps

| Step | Description |
|------|-------------|
| 1. Client Hello | Client sends supported TLS version, cipher suites, and Client Random |
| 2. Server Hello | Server responds with selection and Server Random, sends certificate |
| 3. Certificate Validation | Client authenticates server's certificate |
| 4. Client Key Exchange | Client sends encrypted pre-master secret |
| 5. Server Decrypts Secret | Server decrypts pre-master secret |
| 6. Session Key Creation | Both create master secret and session keys |
| 7. Client Change Cipher Spec & Finished | Client signals encryption readiness, sends finished message |
| 8. Server Change Cipher Spec & Finished | Server signals encryption readiness, sends finished message |
| 9. Encrypted Communication | Secure data exchange begins |

## Notes

- TLS 1.3 simplifies this process by reducing round trips and eliminating some steps, improving speed and security.
- The TLS handshake ensures confidentiality, integrity, and authentication between client and server during communication.

This handshake process is fundamental in securing HTTPS connections, email servers, VPNs, and other systems requiring secure communication over networks.

---
---
# TLS 1.3 Handshake Process

The TLS 1.3 handshake is a significantly improved version of the TLS protocol that establishes a secure, encrypted connection between a client and a server. This handshake is faster, more secure, and simpler than previous versions, requiring only 1 round-trip time (1-RTT) for a full handshake. The protocol removes legacy features and mandates perfect forward secrecy. Below is a detailed explanation of the TLS 1.3 handshake process:

## TLS 1.3 Handshake Process Steps

1. **Client Hello**
   - The client initiates the handshake by sending a "Client Hello" message to the server.
   - **TLS Version**: The client indicates it supports TLS 1.3 (though for backward compatibility, this field shows TLS 1.2, with the actual version in the "supported_versions" extension).
   - **Client Random**: A 32-byte random value generated by the client, used later in key derivation to ensure uniqueness of the session.
   - **Cipher Suites**: A list of supported AEAD cipher suites (e.g., TLS_AES_128_GCM_SHA256, TLS_AES_256_GCM_SHA384, TLS_CHACHA20_POLY1305_SHA256).
   - **Key Shares**: The client preemptively generates ephemeral key pairs for Diffie-Hellman key exchange and includes the public keys for its preferred groups (e.g., x25519, secp256r1). This is crucial for achieving 1-RTT - the client guesses which group the server will choose.
   - **Supported Groups**: Lists all the elliptic curves or DH groups the client supports.
   - **Signature Algorithms**: Indicates which signature algorithms the client can verify for certificates.
   - **Pre-Shared Key (PSK)**: If resuming a previous session, the client includes PSK identities and a PSK key exchange mode. This enables 0-RTT data transmission.
   - **Early Data**: If using 0-RTT, the client can send encrypted application data immediately with this hello message.

2. **Server Hello**
   - The server analyzes the Client Hello and responds with its choices.
   - **TLS Version Confirmation**: Confirms TLS 1.3 in the supported_versions extension.
   - **Server Random**: A 32-byte random value generated by the server.
   - **Cipher Suite Selection**: The server selects one cipher suite from the client's list.
   - **Key Share**: The server selects one of the client's offered groups and provides its own ephemeral public key for that group. If the client didn't offer a key share for the server's preferred group, the server sends a "HelloRetryRequest" asking for the correct key share.
   - **Pre-Shared Key**: If resuming a session, the server selects which PSK to use.
   
   **At this point, both parties can calculate the handshake traffic secrets using ECDHE (Elliptic Curve Diffie-Hellman Ephemeral) with the exchanged key shares.**

3. **Server Encrypted Extensions**
   - **Encryption Begins**: All subsequent handshake messages from the server are encrypted using keys derived from the handshake traffic secret.
   - **Extensions Include**:
     - Server Name Indication (SNI) acknowledgment
     - Application Layer Protocol Negotiation (ALPN) selection (e.g., HTTP/2, HTTP/3)
     - Maximum fragment length
     - Other negotiated parameters
   - These extensions provide additional configuration without exposing it to eavesdroppers.

4. **Server Certificate and Certificate Verify**
   - **Certificate Message**: 
     - Contains the server's X.509 certificate chain, starting with the server's certificate and including intermediate certificates up to a trusted root.
     - The certificate includes the server's public key and identity information (domain name, organization).
     - In TLS 1.3, this is encrypted, providing privacy about which certificate is being used.
   
   - **CertificateVerify Message**:
     - The server creates a digital signature over a hash of all handshake messages up to this point.
     - This signature is created using the private key corresponding to the public key in the certificate.
     - The signature proves the server possesses the private key and that the handshake hasn't been tampered with.
     - The signature covers a special context string "TLS 1.3, server CertificateVerify" plus the handshake transcript hash.
   
   - **CertificateRequest (Optional)**:
     - If the server requires client authentication, it sends this message.
     - Specifies acceptable certificate authorities and signature algorithms for client certificates.

5. **Server Finished**
   - The server computes a **Finished MAC** (Message Authentication Code) using the handshake traffic secret.
   - This MAC is computed over a hash of all handshake messages sent and received up to this point.
   - The Finished message provides key confirmation and binds all handshake messages together.
   - After sending this, the server can derive the **server application traffic secret** for encrypting application data.

6. **Client Processing and Response**
   - **Certificate Verification**: The client verifies the server's certificate chain against its trusted root certificates and checks the CertificateVerify signature.
   - **Handshake Keys Derivation**: The client derives the same handshake traffic secrets to decrypt and verify server messages.
   
   **Client Certificate (If Requested)**:
   - If the server requested client authentication, the client sends:
     - **Certificate**: The client's certificate chain
     - **CertificateVerify**: A signature over the handshake transcript using the client's private key
   
   - **Application Traffic Secrets**: The client derives both client and server application traffic secrets from the handshake traffic secret and the handshake transcript.

7. **Client Finished**
   - The client sends its own **Finished MAC** computed over the complete handshake transcript.
   - This message is encrypted with the handshake traffic secret.
   - Once sent, the client transitions to using application traffic keys for all subsequent data.
   - The server verifies this MAC to ensure the handshake completed successfully without tampering.

8. **Application Data Exchange**
   - Both parties now have matching **application traffic secrets** (separate keys for client-to-server and server-to-client directions).
   - These keys are used for AEAD (Authenticated Encryption with Associated Data) encryption of all application data.
   - The handshake is complete, and secure bidirectional communication begins immediately.
   - **Key Update**: TLS 1.3 supports updating traffic keys during the connection for additional security.

## Key Derivation Process in TLS 1.3

```
             Client Random + Server Random + ECDHE Shared Secret
                                    |
                                    v
                        Extract → Early Secret
                                    |
                                    v
                        Derive → Handshake Secret
                                    |
                        +-----------+-----------+
                        |                       |
                        v                       v
            Client Handshake Traffic   Server Handshake Traffic
                   Secret                    Secret
                        |
                        v
                    Master Secret
                        |
            +-----------+-----------+
            |                       |
            v                       v
    Client Application         Server Application
    Traffic Secret             Traffic Secret
```

## Summary Table of TLS 1.3 Handshake Steps

| Step | Description | Key Elements |
|------|-------------|--------------|
| 1. Client Hello | Client initiates with capabilities and key shares | TLS 1.3, cipher suites, Client Random, ECDHE key shares, extensions |
| 2. Server Hello | Server responds with selections and key share | Cipher selection, Server Random, ECDHE key share, derives handshake keys |
| 3. [Encrypted] Extensions | Server configuration parameters | SNI, ALPN, other negotiated parameters - all encrypted |
| 4. [Encrypted] Certificate + Verify | Server authentication | X.509 certificate chain + signature over handshake |
| 5. [Encrypted] Server Finished | Server's MAC of handshake | HMAC confirming handshake integrity |
| 6. [Encrypted] Client Certificate | Client authentication if requested | Client certificate + signature (optional) |
| 7. [Encrypted] Client Finished | Client's MAC of handshake | HMAC confirming handshake integrity |
| 8. Application Data | Encrypted communication | Bidirectional encrypted data using application traffic keys |

## Notes

- **1-RTT Handshake**: TLS 1.3 completes in one round trip, making it approximately 100-200ms faster than TLS 1.2.
- **0-RTT Resumption**: For resumed sessions, TLS 1.3 supports 0-RTT data, allowing clients to send encrypted data immediately with the Client Hello, though with some security trade-offs (replay attacks).
- **Perfect Forward Secrecy**: All TLS 1.3 cipher suites provide PFS through ephemeral Diffie-Hellman, meaning past sessions remain secure even if long-term keys are compromised.
- **Simplified Cipher Suites**: TLS 1.3 uses only 5 AEAD cipher suites, with key exchange and signature algorithms negotiated separately.
- **Enhanced Privacy**: Server certificate and all handshake messages after Server Hello are encrypted, hiding certificate details from passive observers.
- **Removed Legacy Features**: No more RSA key exchange, CBC mode ciphers, compression, renegotiation, or non-AEAD ciphers.
- **Downgrade Protection**: Special values in Server Random prevent downgrade attacks to older TLS versions.

This streamlined handshake process is fundamental in securing modern HTTPS connections, providing faster page loads, enhanced security, and better privacy protection for users worldwide.

---
---