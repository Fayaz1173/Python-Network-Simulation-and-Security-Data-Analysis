# Socket Programming 

## Server

```python
import socket

HOST = "127.0.0.1"
PORT = 12345

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server.bind((HOST, PORT))
server.listen(1)

print("Server is waiting for a connection...")

client_socket, address = server.accept()
print("Connected to:", address)

while True:
    
    client_message = client_socket.recv(1024).decode()
    
    if client_message.lower() == "exit":
        print("Client disconnected.")
        break

    print("Client:", client_message)

    server_message = input("Server: ")
    client_socket.send(server_message.encode())

    if server_message.lower() == "exit":
        break

client_socket.close()
server.close()
```

## Client

```python
import socket

HOST = "127.0.0.1"
PORT = 12345

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

client.connect((HOST, PORT))

print("Connected to server")

while True:
   
    message = input("Client: ")
    client.send(message.encode())

    if message.lower() == "exit":
        break

    response = client.recv(1024).decode()
    print("Server:", response)

    if response.lower() == "exit":
        break

client.close()
```

## Response after connecting client to server

<img width="1407" height="246" alt="Screenshot 2026-06-19 125346" src="https://github.com/user-attachments/assets/7af4a0ea-332b-4ab8-948b-22e9bb02dd11" />


## Demonstrate bidirectional communication (client sends message → server
responds).

### Communication Flow

```
Client                         Server

Hello Server  ------------->   Receives message
Receives reply <-------------  Hello Client

How are you? ------------->    Receives message
Receives reply <-------------  I am fine
```

This demonstrates **two-way (bidirectional) TCP communication** using Python sockets, where the **client sends messages and the server responds continuously until the connection is closed**.

## Communication Flow

```
        TCP Connection
              |
              v

Client                         Server
  |                               |
Create Socket                Create Socket
  |                               |
Connect --------------------> Listen & Accept
  |                               |
Send Message ----------------> Receive Message
  |                               |
Receive Response <------------ Send Response
  |                               |
Send Another Message --------> Receive Again
  |                               |
          (repeats until exit)
  |                               |
Close Connection            Close Connection
```

### In simple words:

- The **server waits** for a connection.
- The **client connects** to the server.
- The **client sends a message**.
- The **server receives it and sends a reply**.
- This exchange continues in a loop until the connection is closed.

This is the basic working principle behind chat applications, remote connections, and many client-server applications using **TCP socket programming**.
