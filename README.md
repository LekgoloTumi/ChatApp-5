# Project Overview: Loqui Chat Application

Loqui is a simple, multi-client chat application that enables users to communicate in real-time over a network. The project consists of two main components: a client and a server. The server handles connections from multiple clients, manages messages, and broadcasts them to all connected users. The client sends messages to the server, listens for incoming messages, and displays them to the user.
# 1. Server-Side (Server Script)

The server script handles the core operations of the chat application. It listens for incoming client connections, manages communication between multiple clients, and handles user actions such as joining or leaving the chat.
Key Components:

    Socket Setup:

        A server socket is created using socket.socket(socket.AF_INET, socket.SOCK_STREAM), which allows the server to communicate over the network using TCP/IP (streaming data).

        The server binds to a specific IP address (socket.gethostname()) and port (32973), which allows clients to connect to it.

    Connection Handling:

        The server listens for incoming client connections using server.listen().

        Once a client connects, a new thread is spawned for each client using threading.Thread. This allows the server to handle multiple clients concurrently.

    Client Registration:

        Upon connection, the server expects a username from the client. The username is checked against a list of existing usernames. If the username is already taken, the client is disconnected.

        If the username is accepted, the server adds the client to the list of connected clients and broadcasts a message informing other users that the new user has joined.

    Message Handling:

        Once a user is connected, the server continuously listens for incoming messages from the client. When a message is received, it is broadcast to all connected clients using the broadcast() function.

        If a client sends the special !DISCONNECT message, the server will broadcast a message saying the user has left the chat, remove the client from the list of connected users, and close the connection.

    Thread Safety:

        To ensure that multiple threads (handling different clients) don’t simultaneously modify shared resources like the list of connected clients or usernames, a lock (threading.Lock) is used.

    Error Handling:

        The server has robust error handling, particularly for connection issues, and it ensures that disconnected clients are properly removed from the active client list.

# 2. Client-Side (Client Script)

The client script is responsible for interacting with the user and sending messages to the server. It also listens for incoming messages from other users and displays them in real-time.
Key Components:

    Socket Connection:

        The client connects to the server using the server’s IP address and port (35.208.151.24 and 32973). It uses client.connect(ADDR) to establish a connection.

    Username Registration:

        When the client starts, it prompts the user to enter a username. The username is sent to the server. If the username is already taken, the client is notified and asked to choose a different username.

    Message Sending:

        Once connected, the client enters a loop where it can type and send messages to the server. The client sends messages to the server using the send() function. Messages are then broadcast to all other clients by the server.

    Receiving Messages:

        The client listens for incoming messages from the server in a separate thread. This is done to allow the client to both send and receive messages simultaneously without blocking. The receive() function listens to the server for new messages and prints them to the console.

    Disconnecting:

        When the user decides to exit the chat, they can type !DISCONNECT to signal the server that they wish to leave. This results in the server broadcasting that the user has left the chat, and the client closes the connection.

    Error Handling:

        The client has error handling to deal with failed message sending or receiving due to connection issues.

# How Loqui Works:

    Server Initialization:

        The server is started, and it listens for incoming connections on the specified address (35.208.151.24) and port (32973).

    Client Connection:

        Clients start the application, providing a username that they wish to use.

        The client connects to the server and registers their username. If the username is valid (not already taken), the client is added to the list of active clients, and a broadcast message is sent to other clients announcing the new arrival.

    Real-Time Communication:

        Clients can send messages, which are broadcast to all connected clients by the server. The client listens for incoming messages in real-time and displays them on the user’s console.

    User Disconnection:

        When a user disconnects (by typing !DISCONNECT), the server broadcasts a message saying that the user has left the chat, removes the client from the list of active clients, and closes the connection.

    Concurrency:

        The server handles multiple clients simultaneously using threads. Each client runs in its own thread, allowing multiple clients to send and receive messages at the same time without blocking the server or other clients.

# Potential Future Enhancements:

    Private Messaging: Allow clients to send direct messages to other users, rather than broadcasting to all.

    User Authentication: Implement user authentication (e.g., password protection) to ensure only authorized users can connect to the server.

    GUI: Add a graphical user interface (GUI) for the client using libraries such as Tkinter or PyQt, which would improve user experience.

    Chat Rooms: Create chat rooms or channels where users can join specific groups based on interests.

# Conclusion:

Loqui is a simple yet functional multi-client chat application built using Python’s socket library and threading. The server handles client connections, broadcasts messages to all users, and ensures thread safety with locks. The client interacts with the user, sending messages to the server and displaying incoming messages in real-time. The application demonstrates essential concepts like network communication, multithreading, and basic error handling.
