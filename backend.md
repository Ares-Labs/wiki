# Introduction with the backend

The backend server is a Java application that uses the [Vert.x](https://vertx.io/) framework. All information of the
client is communicated over a websocket connection through the vertx event bus. The server is responsible for managing
the database connection and communicating with the client.

## Handling events

The server has a custom event handler which is a singleton that handles all events that are sent to the server. This
class *(`be.howest.ti.mars.logic.domain.EventHandler``)* manages all messages that are received.

If you want to add a new event, you can use the `addEventHandler` method, the first argument is the specific type to
handle, and the second argument is the method that should handle the event.

```java
EventHandler.getInstance().addEventHandler("my-event", (JsonObject data) -> {
    // Handle the event
    return new StatusMessageEvent("my-event");
});
````

### Handling requests

If a query is sent to the server, a `requestIdentifier` value can be added, this value is not present on the object data
your event handler receives, and it gets automatically added to the response. This allows the client to know which query
the response is for.

## Managing sessions

Right now, every client has to send a message with a type of `session` *(`addNewSession` in the `MarsRtcBridge` file)*.
And the channel all messages are sent on is determined by the client their id. But the addNewSession method can
initialize set the channel to what you'd prefer.

## Sending messages

All responses must implement the `be.howest.ti.mars.web.bridge.SocketResponse` interface. There are already some
pre-made classes that get used. The returned value should be of the `SocketResponse` type.

### Common types

- `DataEventResponse` - Used to send data to the client
- `StatusMessageEventResponse` - Used to send a status message to the client
- `ErrorMessageEventResponse` - Used to send an error message to the client
- `SuccessEventResponse` - Used to send a success message to the client

## Performing database operations

The backend server uses an [H2 database](https://www.h2database.com/html/main.html) to store all data
The `be.howest.ti.mars.logic.data.MarsH2Repository` file manages all database operations. This file contains an enum
called `Queries` which is a single place where all queries are defined which can be used in the `MarsH2Repository`
class.
