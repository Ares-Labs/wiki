# Introduction to the Client Wrapper

Welcome to the client wrapper!

The client wrapper is a tool that allows you to communicate with the server in a convenient and efficient manner. There
are two ways in which the client wrapper can communicate with the server: using `execute` and using events.

Using `execute` allows you to send a query to the server and receive a single response when that query is received. This
is similar to how a normal HTTP request works, but it still flows over the vert.x event bus.

Using events allows you to subscribe to a specific event on the server. When that event occurs on the server, it will
trigger a message to be sent to the client. This allows for ongoing communication between the server and the client, as
multiple messages can be sent in response to a single event.

We hope you find the client wrapper to be a useful and powerful tool for communicating with the server. If you have any
questions or need further assistance, don't hesitate to reach out to our team. We are here to help you make the most of
this tool.

# Waiting for connection

The Gateway class in src/utils/events has an onReady event that you can use to ensure that the connection to the server
is established before you send any data. This is necessary because the client wrapper uses websockets to communicate
with the server, and it must first establish a connection and a custom session before it can send any data. To use the
onReady event, you can pass a callback function as an argument. The callback will be executed when the connection is
ready, or if the connection is already established, it will be executed immediately

```js
import Gateway from './utils/events';

Gateway.onReady(() => {
    console.log('Connection established!');
});
```

# Querying the Server

The `execute` method allows you to query the server. This method requires a valid query type as its first argument and
the
data to be sent to the server as its second argument.

## Adding a query type

Adding a query type is fairly simple, at te top of the `src/utils/events.js` file, there should be a JSDoc type
definition of the `Queries` type, add your new type there. This type definition is linked to the `QUERY_TYPES` object,
which is where you should add your query type (as defined in the type definition and add the associated event type
value [this is the value that gets used by the server to identify a query type]). Please do note that within
the `QUERY_TYPE` you do not need to prefix it with `queries.`, as this is done automatically.

## Executing a query

The `Gateway` class is responsible for managing the connection to a database, including sending queries and waiting for
responses. It uses a custom implementation for communication over an existing websocket connection. The `execute` method
allows you to send a query and return a promise that will resolve with the response from the server

```js
import Gateway from './utils/events';

/**
 * A function that retrieves a list of users from a database.
 *
 * @param {string} search - A search term to filter the list of users.
 * @returns {Promise} A promise that resolves with the list of users.
 */
const getUsers = async (search) => await Gateway.execute(Gateway.queries.GET_USERS, {search});
```

# Listening for events

Events are server sent messages that can occur multiple times. These are very useful for live data without having to
poll the server.

## Adding an event type

Just like adding a new query type, adding an event type is also really easy to do. We also have to add the event type to
the `Events` type definition, and add your type to the `EVENT_TYPE` const. Just like the `QUERY_TYPE` const, you do not
need to prefix the event type with `events.`.

## Listening for an event

You can listen for an event by using the `subscribe`, this takes 2 arguments. The first argument must be a valid event
type and the second argument is a callback function that will be called when the event occurs.

```js
import Gateway from './utils/events';

/**
 * A function that is called when a drone is dispatched.
 *
 * @param {object} data - Data associated with the dispatched drone.
 */
const onDroneDispatched = (data) => {
    // We know that a drone has been dispatched, so we can do something with the data
};

// Listen for the DRONE_DISPATCHED event and call onDroneDispatched when it occurs
Gateway.subscribe(Gateway.events.DRONE_DISPATCHED, onDroneDispatched);
```