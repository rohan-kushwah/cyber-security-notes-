### What is the Three-Way Handshake?

The **three-way handshake** is the procedure used by the TCP protocol to establish a reliable connection between a client and a server. It ensures that both devices are ready for communication and that the data can be sent without errors.

### Steps in the Three-Way Handshake

1. **SYN (Synchronize)**
    - The client sends a **SYN** message (a request to start the connection) to the server. This message indicates the client's initial sequence number.
2. **SYN-ACK (Synchronize-Acknowledgment)**
    - The server responds with a **SYN-ACK** message, which both acknowledges the client's request (via the ACK part) and also sends its own sequence number.
3. **ACK (Acknowledgment)**
    - The client responds with an **ACK** message to acknowledge the server's SYN-ACK message. This completes the handshake.

Once these three steps are completed, the connection is established, and data transfer can begin.

---

### Example: A Simple Three-Way Handshake

#### Scenario: A client (A) wants to establish a connection with a server (B).

1. **Step 1: Client (A) sends SYN to Server (B)**
    
    - A sends a message:  
        **SYN: Seq = 1000**  
        This means A wants to start the communication and indicates its initial sequence number is 1000.
2. **Step 2: Server (B) responds with SYN-ACK**
    
    - B receives the SYN from A, and responds with:  
        **SYN-ACK: Seq = 2000, Ack = 1001**  
        B's sequence number is 2000, and it acknowledges the client's sequence number by sending `Ack = 1001` (the next expected number from A).
3. **Step 3: Client (A) sends ACK to Server (B)**
    
    - A receives the SYN-ACK from B and responds with:  
        **ACK: Seq = 1001, Ack = 2001**  
        This message acknowledges the server's sequence number and confirms the connection.

---

### After the Three-Way Handshake

At this point, the connection is established, and both sides can start transmitting data.

#### Summary:

- **Client sends SYN** → **Server responds with SYN-ACK** → **Client acknowledges with ACK**.
- This ensures both the client and server are synchronized and ready for communication.