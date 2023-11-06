## Protocol documentation

Here, we will be talking about the protocol used in the custom client and server.
The protocol is based on the TCP and UDP protocol.

### TCP Protocol

The TCP protocol is used to connect the client to the server and to validate the client.
It is also used to send important informations because it is more reliable than the UDP protocol.

```cpp
enum class tcpProtocol : uint8_t
{
    Udp,
    StartRoom,
    SetName,
};
```
As you can see, the tcp protocol is really simple.
It only contains two commands:
    - Udp: this command is used to tell the client that the server is starting a udp protocol and that the client has to connect to the udp socket.
        The port and the ip of the udp socket are sent in the message.
    - StartRoom: this command is used to tell the server that the client want to start a server, this can be used to let clients create their own room.
    - SetName: this command is used to tell the server the client's name.

But those commands are not the only purpose of the tcp protocol.
It is also used to create and manage the lobby.

```cpp
enum class tcpProtocol : uint8_t
{
    LobbyCreate,
    LobbyJoin,
    LobbyLeave,
    LobbyStart,
    LobbySetName,
    LobbyChangeGame,
    LobbyRemoved,

    LobbyCreated,
    LobbyJoined,
    LobbyLeft,
    LobbyStarted,
    LobbyNameSet,
};
```
One of the most important command is the LobbyCreate command.
This command is used to create a lobby.
The server will then send a LobbyCreated message to the client.
This message contains the id of the lobby and the name of the client.

Then, the client can join the lobby using the LobbyJoin command.
The server will then send a LobbyJoined message to the client and a LobbyJoined message to all the other clients so they can update their lobby list.

The owner of the lobby can then start the lobby using the LobbyStart command.
The server will then send a LobbyStarted message to all the clients so they can update their lobby list.
and the server will also start the game.


#### Packages

```cpp
    struct Message {
        MessageHeader<T> header;
        std::vector<uint8_t> body;
    }
```
The header of the message contains the type of the message and the size of the body.
The body contains the data that we want to send.
It is sent using a vector of uint8_t.
The message can contain any type of data of any size.

### UDP Protocol

So, now that game is started and the client is connected to the udp socket,
we can use the udp protocol to send and receive datas faster than with the tcp protocol.

```cpp
enum class udpProtocol : uint8_t
{
    enum class udpProtocol : uint8_t
{
    ConnectedPlayer,
    CreatePlayer,
    CreateEnnemy,
    CreateBullet,
    Destroy,
    PlayerMove,
    Resync,

    PongPlayerAskMove,
    PongPlayerMove,
    PongBallMove,
    PongScore,
    PongResync,
};
};
```
As you can see, the udp protocol is more complex than the tcp protocol.
This is because we need to send more informations.

In this example, the server owns every game objects.
So the client has to ask the server to create a game object.
And the server has to send the informations of the game objects to the client.
It is also necessary to send the informations of the players to the other players.

By using this server side architecture, we can avoid cheating.
And we can also avoid the problem of synchronisation between the clients.
The server has the official version of the game, and the clients are just displaying it.

So now that we know why we need to send all those informations, let's see how we do it.

Let's take the example of the player.
At the beginning of the game, the server send to each player, a `CreatePlayer` message.
This message contains the informations of the player.
Then, each time the player moves, the client send a `PlayerMove` message to the server.
The server will then send a `PlayerMove` message to all the other players.

And so on for the other game objects.

It is interesting to note that the Destroy and resync messages are generic.
This is because the client does not need to know what is destroyed or resynced.
It just needs to know the id of the object that is destroyed or resynced.
This id is shared between the server and the client.

The resync command is used to resync the client with the server.
It is used x times per second to make sure that the client is up to date.

#### Packages

```cpp
    #define MSG_SIZE 1024

    struct MessageUdp {
        MessageHeaderUdp<T> header;
        std::array<uint8_t, MSG_SIZE - sizeof(header)> body = {};
    }
```

The header of the message contains the type of the message.
The body contains the data that we want to send.
Here we use an array of uint8_t because the size of the body is fixed.
Each package has a size of 1024 bytes.
It is necessary to use a fixed size because the udp protocol does not guarantee the order of the packages.