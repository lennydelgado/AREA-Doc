### Descpription

This library is used to create a server and a client.
it provides a simple interface to send and receive any kind of data.

the network interface is based on the [asio](https://think-async.com/Asio/) library.

it is working on all platforms and is fully costumisable.


### Usage

To use the Flib, you have to create custom DataTypes.
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

Now, you can send datas using the "message" class

For the server, you can use messageClient
and in the client ***A compléter.***

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
As you can see we filled the message in the order **[a -> b -> data]**

And then we extracted the data in the order **[data -> b -> a]**

This is because the message structure works like a pile. So the last item we put in the message will be the first one we extract.

### Client

### Server

```cpp
Server::Server(int port);
```
this method will create a server on the given port

## Server::start()

```cpp
bool Server::start();
```
this method will start the server

if an error occurs it will throw an exception and print the error message on the error output

then it will return false

otherwise it will return true and the server will be running


## Server::stop()

```cpp
void Server::stop();
```
this method will stop the server


