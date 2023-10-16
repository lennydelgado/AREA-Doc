# Descpription

This library is used to create a server and a client.
it provides a simple interface to send and receive any kind of data.

the network interface is based on the [asio](https://think-async.com/Asio/) library.

it is working on all platforms and is fully costumisable.


# Usage

To use the Flib, you have to create custom DataTypes.
Those DataTypes will be used to identify the type of data that you want to send.

You can add or modify those DataTypes in the file `protocol.hpp`

Here is an example:

```cpp
    enum class CustomMsgTypes : uint32_t
    {
        PlayerData,
        FireBullet,
        MovePlayer,
        PlayerHealth
    };
```
This is basicly an enum that you can use to implement different behaviors.

Those DataTypes will be used in the Networking implementation. Go to Data Transfer for more details.

Now, you can send datas using the `message` class

For the server, you can use the methode `onMessage` to implement what you want to do when you receive a message.
    
```cpp
    virtual void onMessage(std::shared_ptr<connection<T>>& client, message<T>& msg)
```

## message Class

```cpp
template <typename T>
    struct messageHeader
    {
        T id{}; // {} = default value / constructor for class
        size_t size = 0;
    };

template <typename T>
    struct message
    {
        messageHeader<T> header;
        std::vector<uint8_t> body;
    };
```
This is the basic structure of a message.
The `messageHeader` contains the custom data type that we created earlier.
Thanks to the `id{}` syntaxe, it can be any type of data, even struct or class (in those cases the constructor will be called by default).

# Create a message

```cpp
    net::message<CustomMsgTypes> msg;
    msg.header.id = CustomMsgTypes::PlayerData;
```
# Fill a message

```cpp
    int a = 5;
    float b = 3.14f;
    struct
    {
        int a;
        int b;
    } data[5];

    msg << a << b << data;
```

# Extract data from a message

```cpp
    int a;
    float b;
    struct
    {
        int a;
        int b;
        float c;
    } data[5];

    msg >> data >> b >> a;
```
As you can see we `filled` the message in the order **[a -> b -> data]**

And then we `extracted` the data in the order **[data -> b -> a]**

This is because the message structure works like a pile. So the last item we put in the message will be the first one we extract.

### Client

```cpp
client_interface() = default;
```
This method will `create` a client

## Client::connect()

```cpp
bool connect(const std::string& host, const size_t port)
```

This method will `connect` the client to the given host and port\
Returns true if the connection is successful, false otherwise


### Server

```cpp
Server::Server(uint16_t port);
```
this method will `create` a server on the given port

## Server::start()

```cpp
bool Server::start();
```
this method will `start` the server

if an error occurs it will throw an exception and print the error message on the error output

then it will return false

otherwise it will return true and the server will be running


## Server::stop()

```cpp
void Server::stop();
```
this method will `stop` the server

## Server::messageClient()

```cpp
void messageClient(std::shared_ptr<connection<T>> client,
                   const message<T>& msg)
```

This method will `sends a message` to a connected client.
If the client is not connected, it removes the client from the list of connections.

## Server::messageAllClients()

```cpp
void messageAll(const message<T>& msg)
```

This method will `sends a message` to all connected clients.
If a client is not connected, it will be removed from the list of connections.

## Server::update()

```cpp
void update(size_t maxMessages = -1)
```

This method will `update` the server by processing incoming messages
It take one argument maxMessages.\
Default value is -1, which means all available messages will be processed.

## Server::onMessage()

```cpp
virtual void onMessage(std::shared_ptr<connection<T>>& client, message<T>& msg)
```

This method is called when a `message is received`.\
It is a virtual method, so you can override it to implement your own behavior.

## Udp Server

```cpp
class customServer : public net::server<tcpProtocol>
```

Custom server class that inherits from the server class and uses the UDP protocol.\
You can find the same methods as the server class.\
The only difference is that the custom server class uses the `TCP` protocol.

## customServer::startLobby

```cpp
void startLobby();
```

This method will `start` the lobby.