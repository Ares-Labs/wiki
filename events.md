# What are events?

Events are ALL messages that are sent over the websocket connection.

## Internal events

These events are internal events that are used to manage how and what the server should send to the client. All of these
events are just sent from the client to the server without any prefix.

### `session`

Initiate a new session with the server. This event is sent automatically when the client connects to the server.

**data**

- id: String - The client id, which is used to identity the client and send the messages over the channel.

**returns**: `StatusMessageEventResponse`

### `subscribe`

Add a new subscription to the server. If a client wants to subscribe to a new event, it must send a `subscribe` event.
Once this is sent the server can emit a subscription event to the client when the event occurs.

**data**

- clientId: String - The client id, which is used to identity the client and send the messages over the channel.
- id: String - The id of the event to subscribe to.

**returns**: `StatusMessageEventResponse`

## Queries

Queries represent a request to the server to perform a specific action. For example some queries can be used to fetch
data, update data or perform other actions. All queries are prefixed with `queries.`.

### User related queries

#### `queries.get-user`

Get a user from the database.

**data**

- userId: String - The id of the user to get.

**returns**: `DataEventResponse`

- id: String - The id of the user.
- fullName: String - The username of the user.

#### `queries.get-users`

Get a list of users from the database.

**data**

- limit: int - The maximum number of users to return. Default is 10.
- offset: int - The number of users to skip before returning. Default is 0.
- search: String - A string to search for in the usernames. Default is an empty string.

**returns**: `DataEventResponse`

- users: List - A list of users. Each user has the following properties:
    - id: String - The id of the user.
    - fullName: String - The username of the user.

#### `queries.get-user-properties`

Get the properties of a user from the database.

**data**

- userId: String - The id of the user whose properties to get.

**returns**: `DataEventResponse`

- properties: List - A list of the user's properties. Each property has the following fields:
    - id: String - The id of the property.
    - name: String - The name of the property.
    - value: String - The value of the property.

### Equipment related queries

#### `queries.dispatch-drone`

Dispatch a drone from a property.

**data**

- propertyId: int - The id of the property from which to dispatch the drone.

**returns**: `DataEventResponse` or ErrorEventResponse

- If successful:
    - droneId: int - The id of the dispatched drone.
- If unsuccessful:
    - error: String - An error message explaining the failure.

**emits**: `events.drone-dispatched`

- droneId: int - The id of the dispatched drone.

#### `queries.get-dispatched-drones`

Get a list of dispatched drones from the database.

**data**

- search: String - A string to search for in the drone names. Default is an empty string.
- limit: int - The maximum number of drones to return. Default is 10.
- offset: int - The number of drones to skip before returning. Default is 0.

**returns**: `DataEventResponse`

- drones: List - A list of dispatched drones. Each drone has the following properties:
    - id: String - The id of the drone.
    - description: String - The description of the drone.
    - propertyId: int - The id of the property from which the drone was dispatched.

#### `queries.get-free-drones`

Get a list of free drones at a property.

**data**

- propertyId: int - The id of the property for which to get free drones.

**returns**: `DataEventResponse`

- drones: List - A list of the free drones at the property.
    - id: String - The id of the drone.

#### `queries.recall-drone`

Recall a dispatched drone.

**data**

- droneId: int - The id of the drone to recall.

**returns**: `StatusMessageEventResponse`

**emits**: `events.drone-recalled`

- droneId: int - The id of the recalled drone.

### Property related queries

#### `queries.add-property`

Add a new property to the database.

**data**

- location: String - The location of the property.
- tier: int - The tier of the property.
- x: int - The x coordinate of the property.
- y: int - The y coordinate of the property.
- description: String - A description of the property.
- clientId: String - The id of the client who owns the property.

**returns**: `SuccessEventResponse`

**emits**: `events.property-added`

- location: String - The location of the property.
- tier: int - The tier of the property.
- x: int - The x coordinate of the property.
- y: int - The y coordinate of the property.
- description: String - A description of the property.
- clientId: String - The id of the client who owns the property.

#### `queries.remove-property`

Remove a property from the database.

**data**

- propertyId: int - The id of the property to remove.

**returns**: `SuccessEventResponse`

#### `queries.get-property`

Get a property from the database.

**data**

- propertyId: int - The id of the property to get.

**returns**: `DataEventResponse`

- property: Object - The property with the following fields:
    - id: int - The id of the property.
    - location: String - The location of the property.
    - tier: int - The tier of the property.
    - x: int - The x coordinate of the property.
    - y: int - The y coordinate of the property.
    - width: int - The width of the property.
    - height: int - The height of the property.
    - status: String - The status of the property.
    - description: String - A description of the property.

#### `queries.get-property-detailed`

Get a property from the database, including its detailed information.

**data**

- propertyId: int - The id of the property to get.

**returns**: `DataEventResponse`

- property: Object - The property with the following fields:
    - id: int - The id of the property.
    - location: String - The location of the property.
    - tier: int - The tier of the property.
    - tier_name: String - The name of the tier.
    - x: int - The x coordinate of the property.
    - y: int - The y coordinate of the property.
    - width: int - The width of the property.
    - height: int - The height of the property.
    - status: String - The status of the property.
    - description: String - A description of the property.
    - owner: String - the id of the owner of the property
    - owner_full_name: String - the full name of the owner of the property
    - equipment: List - The equipment that is installed on the property
        - id: int - The id of the equipment
        - type: int - The type of the equipment
        - description: String - A description of the equipment
        - name: String - The name of the equipment

#### `queries.get-properties`

Get a list of properties from the database.

**data**

- limit: int - The maximum number of properties to return. Default is 10.
- offset: int - The number of properties to skip before returning. Default is 0.
- search: String - A string to search for in the property locations. Default is an empty string.

**returns**: `DataEventResponse`

- properties: List - A list of properties. Each property has the following fields:
    - id: int - The id of the property.
    - location: String - The location of the property.
    - tier: int - The tier of the property.
    - x: int - The x coordinate of the property.
    - y: int - The y coordinate of the property.
    - status: String - The status of the property.
    - description: String - A description of the property.

#### `queries.change-property-size`

Change the size of a property in the database.

**data**

- propertyId: int - The id of the property to change.
- width: int - The new width of the property in meters.
- height: int - The new height of the property in meters.

**returns**: `SuccessEventResponse`

**emits**: `events.property-size-changed`

- propertyId: int - The id of the property whose size was changed.
- width: int - The new width of the property in meters.
- height: int - The new height of the property in meters.

#### `queries.change-property-coordinates`

Change the coordinates of a property in the database.

**data**

- propertyId: int - The id of the property to change.
- x: int - The new x coordinate of the property.
- y: int - The new y coordinate of the property.

**returns**: `SuccessEventResponse`

**emits**: `events.property-coordinates-changed`

- propertyId: int - The id of the property whose coordinates were changed.
- x: int - The new x coordinate of the property.
- y: int - The new y coordinate of the property.

#### `queries.change-property-status`

Change the status of a property in the database.

**data**

- propertyId: int - The id of the property to change.
- status: String - The new status of the property.

**returns**: `SuccessEventResponse`

**emits**: `events.property-status-change`

- propertyId: int - The id of the property whose status was changed.
- status: String - The new status of the property.

#### `queries.get-pending-properties**`

Get a list of properties with a "PENDING" status from the database.

**returns**: `DataEventResponse`

- properties: List - A list of properties with a "PENDING" status. Each property has the following fields:
    - id: int - The id of the property.
    - location: String - The location of the property.
    - tier: int - The tier of the property.
    - x: int - The x coordinate of the property.
    - y: int - The y coordinate of the property.
    - width: int - The width of the property.
    - height: int - The height of the property.
    - status: String - The status of the property.
    - description: String - A description of the property.

#### `queries.add-equipment`

Add equipment to a property in the database.

**data**

- propertyId: int - The id of the property to which the equipment should be added.
- equipmentType: int - The type of equipment to add.
- description: String - A description of the equipment.

**returns**: `DataEventResponse`

- id: int - The id of the added equipment.

**emits**: `events.property-equipment-change`

- propertyId: int - The id of the property to which the equipment was added.
- equipmentId: int - The id of the added equipment.
- equipmentType: int - The type of equipment that was added.
- description: String - A description of the equipment.

#### `queries.remove-equipment`

Remove equipment from a property in the database.

**data**

- propertyId: int - The id of the property from which the equipment should be removed.
- equipmentId: int - The id of the equipment to remove.

**returns**: `SuccessEventResponse`

**emits**: `events.property-equipment-change`

- propertyId: int - The id of the property from which the equipment was removed.
- equipmentId: int - The id of the removed equipment.

#### `queries.get-equipment`

Get the equipment for a property from the database.

**data**

- propertyId: int - The id of the property whose equipment to get.

**returns**: `DataEventResponse`

- equipment: List - A list of the equipment for the property. Each equipment has the following fields:
    - id: int - The id of the equipment.
    - type: int - The type of the equipment.
    - description: String - A description of the equipment.
    - name: String - The name of the equipment.

#### `queries.change-property-tier`

Change the tier of a property in the database.

**data**

- propertyId: int - The id of the property to change.
- tier: int - The new tier of the property.

**returns**: `SuccessEventResponse`

**emits**: `events.property-tier-changed`

- propertyId: int - The id of the property whose tier was changed.
- tier: int - The new tier of the property.

#### `queries.search-pending-properties`

Search for pending properties in the database.

**data**

- search: String - A search string to filter the results.
- limit: int - The maximum number of results to return.
- offset: int - The number of results to skip.

**returns**: `DataEventResponse`

- properties: List - A list of pending properties that match the search criteria. Each property has the following
  fields:
    - id: int - The id of the property.
    - location: String - The location of the property.
    - tier: int - The tier of the property.
    - x: int - The x coordinate of the property.
    - y: int - The y coordinate of the property.
    - width: int - The width of the property.
    - height: int - The height of the property.
    - status: String - The status of the property.
    - description: String - A description of the property.

#### `queries.search-removal-properties`

Search for properties that are scheduled for removal in the database.

**data**

- search: String - A search string to filter the results.
- limit: int - The maximum number of results to return.
- offset: int - The number of results to skip.

**returns**: `DataEventResponse`

- properties: List - A list of properties that are scheduled for removal and match the search criteria. Each property
  has the following fields:
    - id: int - The id of the property.
    - location: String - The location of the property.
    - tier: int - The tier of the property.
    - x: int - The x coordinate of the property.
    - y: int - The y coordinate of the property.
    - width: int - The width of the property.
    - height: int - The height of the property.
    - status: String - The status of the property.
    - description: String - A description of the property.

#### `queries.request-remove-property`

Request that a property be removed from the database.

**data**

- propertyId: int - The id of the property to remove.

**returns**: `SuccessEventResponse`

**emits**: `events.requested-remove-property`

- propertyId: int - The id of the property that was requested for removal.

#### `queries.get-requested-remove-properties**`

Get a list of properties that have been requested for removal from the database.

**data**: `None`

**returns**: `DataEventResponse`

- properties: List - A list of the requested remove properties. Each property has the following fields:
    - id: int - The id of the property.
    - location: String - The location of the property.
    - clientId: String - The id of the client who owns the property.
    - tier: int - The tier of the property.
    - x: int - The x coordinate of the property.
    - y: int - The y coordinate of the property.
    - width: int - The width of the property.
    - height: int - The height of the property.
    - status: String - The status of the property.
    - description: String - The description of the property.

#### `queries.approve-remove-property`

Approve a request to remove a property from the database.

**data**

- propertyId: int - The id of the property to remove.

**returns**: `SuccessEventResponse`

**emits**: `events.approved-remove-property`

- propertyId: int - The id of the property that was removed.

#### `queries.get-allowed-users`

Get the users that are allowed to access a property from the database.

**data**

- propertyId: String - The id of the property to get allowed users for.

**returns**: `DataEventResponse`

- allowedUsers: Object - The allowed users, the key is the id of the user, and the value is their full name.

#### `queries.add-allowed-user`

Add a user to the list of allowed users for a property.

**data**

- propertyId: String - The id of the property to add the user to.
- userId: String - The id of the user to add.

**returns**: `SuccessEventResponse`

#### `queries.remove-allowed-user`

Remove an allowed user from a property in the database.

**data**

- propertyId: String - The id of the property.
- userId: String - The id of the user to remove.

**returns**: `SuccessEventResponse`

#### `queries.get-alerts`

Get the alerts for a given property from the database.

**data**

- propertyId: String - The id of the property to get the alerts for.

**returns**: `DataEventResponse`

- alerts: Array - An array of alert objects.
    - id: int - The id of the alert.
    - userId: String - The id of the user who triggered the alert
    - timestamp: String - The timestamp of the alert.

#### `queries.add-alert`

Add an alert to a property in the database.

**data**

- propertyId: String - The id of the property to add the alert to.
- userId: String - The id of the user adding the alert.

**returns**: `SuccessEventResponse`

**emits**: `events.alerts`

- propertyId: String - The id of the property that the alert was added to.
- user: String - The id of the user who added the alert.

#### `queries.get-weekly-visitors`

Get a list of visitors to a property in the past week.

**data**

- propertyId: int - The id of the property to get the visitors for.

**returns**: `DataEventResponse`

- visitors: List[int] - A list of user ids representing the visitors to the property.

#### `queries.add-visitor`

Add a visitor to the database.

**data**

- userId: String - The id of the user who is visiting.
- propertyId: int - The id of the property being visited.
- cameraId: int - The id of the camera that recorded the visit.

**returns**: `SuccessEventResponse`

**emits**: `events.visits`

- propertyId: int - The id of the property being visited.
- clientId: String - The id of the user who is visiting.

#### `queries.get-scanned-visitors`

Get a list of scanned visitors for a specific property within a given time range.

**data**

- propertyId: String - The id of the property.
- from: String - The start date of the time range in the format YYYY-MM-DD.
- to: String - The end date of the time range in the format YYYY-MM-DD.

**returns**: `DataEventResponse`

- An array of scanned visitor objects. Each object has the following fields:
    - day: String - The day of the visit in the format YYYY-MM-DD.
    - count: int - The number of visitors on the day.

#### `queries.get-crimes-in-area`

Get the count of crimes in a specific area over the past three days.

**data** `None`

**returns**: `DataEventResponse`

- crimes: [{ day: int, count: int }] - An array of objects representing the number of crimes that occurred on each day.
  The `day` field represents the number of days ago (1 = yesterday, 2 = two days ago, etc.) and the `count` field
  represents
  the number of crimes that occurred on that day.

#### `queries.add-crime`

Add a crime to the database.

**data** `None`

**returns**: `SuccessEventResponse`

**emits**: `events.crimes`

- propertyId: int - The id of the property where the crime took place.
- clientId: int - The id of the client who committed the crime.

#### `queries.get-auth-entries`

Get the authorization entries for a specific property.

**data**

- propertyId: String - The id of the property to get authorization entries for.

**returns**: `DataEventResponse`

- authEntries: Array - An array of authorization entries for the property. Each entry is an object with the following
  fields:
    - user_id: String - The id of the user who is authorized.
    - timestamp: String - The timestamp of the authorization.

#### `queries.add-auth-entry`

Add an authorization entry for a user entering a property.

**data**

- propertyId: String - The id of the property being entered.
- userId: String - The id of the user entering the property.

**returns**: `SuccessEventResponse`

## Subscriptions

### About events

In software development, an event is an action or occurrence recognized by a program that may be handled by the program.
Events allow programs to react to changes or occurrences in real time, without the need to constantly poll the server
for updates. This can be especially useful in cases where data changes frequently or in real time, as it allows the
program to immediately react to these changes. For example, in a web application that displays real-time stock prices,
an event could be triggered whenever the stock price changes, allowing the program to update the displayed price without
the need to constantly poll the server. In this way, events allow programs to be more responsive and efficient, and to
provide a more seamless user experience.

### events.all

Emitted for all events.

### `events.alerts`

Emitted when a new alert is added.

**data**

- propertyId: String - The id of the property where the alert was added.
- user: String - The id of the user who added the alert.

### `events.visits`

Emitted when a new visitor is added.

**data**

- propertyId: int - The id of the property where the visitor was added.
- clientId: String - The id of the visitor who was added.

### `events.crimes`

Emitted when a new crime is added.

**data**

- propertyId: int - The id of the property where the crime occurred.
- clientId: String - The id of the user who reported the crime.

### `events.scanned`

Emitted when a visitor is scanned.

**data**

- propertyId: String - The id of the property where the scan occurred.
- clientId: String - The id of the visitor who was scanned.

### `events.auth-entries`

Emitted when a new entry is added to the authorization log.

**data**

- propertyId: String - The id of the property where the entry was added.
- clientId: String - The id of the user who entered the property.

### `events.property-added`

Emitted when a new property is added.

**data**

- id: int - The id of the property that was added.
- owner: String - The id of the owner of the property.

### `events.property-status-change`

Emitted when the status of a property changes.

**data**

- propertyId: int - The id of the property whose status changed.
- status: String - The new status of the property.

### `events.property-equipment-change`

Emitted when the equipment of a property has been added or removed.

**data**

- propertyId: int - The id of the property whose equipment has changed.
- equipmentId: int - The id of the equipment that was added or removed.
- equipmentType: int - The type of equipment that was added or removed (optional, only present on add).
- description: String - A description of the equipment that was added or removed (optional, only present on add).

### `events.requested-remove-property`

Emitted when a property has been requested to be removed.

**data**

- propertyId: int - The id of the property that was requested to be removed.

### `events.approved-remove-property`

Emitted when a property removal request has been approved.

**data**

- propertyId: int - The id of the property that was approved to be removed.

### `events.drone-dispatched`

Emitted when a drone has been dispatched to a property.

**data**

- propertyId: int - The id of the property to which the drone was dispatched.
- droneId: int - The id of the drone that was dispatched.

### `events.drone-recalled`

Emitted when a drone has been recalled from a property.

**data**

- propertyId: int - The id of the property from which the drone was recalled.
- droneId: int - The id of the drone that was recalled.

### `events.property-coordinates-changed`

Emitted when the coordinates of a property have been changed in the database.

**data**

- propertyId: int - The id of the property whose coordinates were changed.
- x: int - The new x coordinate of the property.
- y: int - The new y coordinate of the property.

### `events.property-size-changed`

Emitted when the size of a property has been changed in the database.

**data**

- propertyId: int - The id of the property whose size was changed.
- size: int - The new size of the property.

### `events.property-tier-changed`

Emitted when the tier of a property has been changed in the database.

**data**

- propertyId: int - The id of the property whose tier was changed.
- tier: int - The new tier of the property.
