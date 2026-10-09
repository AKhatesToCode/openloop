# OPENLOOP — MVP API Contract

Base URL: `http://localhost:3001/api`

All requests and responses use JSON unless otherwise specified.

## Data Model

IDs are strings. Dates use ISO 8601 strings.

### User

* `id: string`
* `name: string`
* `interests: string[]`

### Room

* `id: string`
* `title: string`
* `description: string`
* `topicIds: string[]`
* `hostId: string`
* `participantIds: string[]`
* `status: "live" | "ended"`
* `parentRoomId: string | null`
* `createdAt: string`
* `endedAt: string | null`

### TrailItem

* `id: string`
* `roomId: string`
* `authorId: string`
* `type: "takeaway" | "question"`
* `content: string`
* `createdAt: string`

### Topic

* `id: string`
* `name: string`

### Follow

* `userId: string`
* `topicId: string`

Database collections: `users`, `rooms`, `trailItems`, `topics`, `follows`.

## Endpoints

### Health and users

* `GET /health` — returns `{ "status": "ok" }`.
* `GET /users` — returns the user array.

### Rooms

* `GET /rooms` — returns rooms. Optional query parameters: `status=live|ended`, `topicId=<id>`.
* `GET /rooms/:id` — returns room details, participants, and trail items.
* `POST /rooms` — creates a room.

Create-room body:
`{ "title": string, "description": string, "topicIds": string[], "hostId": string }`

The server generates the ID and timestamps, sets `status` to `live`, sets `endedAt` to null, sets `parentRoomId` to null, and adds the host to `participantIds`.

* `POST /rooms/:id/join` — joins the active demo user to a live room.
* `POST /rooms/:id/leave` — removes the active demo user from a room.
* `POST /rooms/:id/end` — ends a room and sets `endedAt`.

### Room trails

* `POST /rooms/:id/trail` — saves a takeaway or question.

Body:
`{ "authorId": string, "type": "takeaway" | "question", "content": string }`

The server generates the ID and timestamp.

* `GET /users/:id/trails` — returns that user's trail items.

### Follow-up rooms

* `POST /rooms/:id/follow-up` — creates a follow-up room linked to the source room.

Body:
`{ "title": string, "description": string, "topicIds": string[], "hostId": string }`

The server sets `parentRoomId` to the source room ID. It generates all other system-managed fields.

### Topics and follows

* `GET /topics` — returns all topics.
* `GET /users/:id/follows` — returns the user's followed topics.
* `PUT /users/:id/follows/:topicId` — follows a topic.
* `DELETE /users/:id/follows/:topicId` — unfollows a topic.

## Response and Error Rules

* Use HTTP 200 for successful reads and updates.
* Use HTTP 201 for resource creation.
* Use HTTP 400 for invalid input.
* Use HTTP 404 for missing resources.
* Use HTTP 409 for state conflicts.
* Errors return `{ "error": "Human-readable message" }`.

## Validation Rules

* Required strings cannot be empty.
* Referenced users, rooms, and topics must exist.
* Ended rooms cannot be joined.
* Only participants can save trail items.
* Trail types must be valid.
* Follow-up rooms require an existing source room.
* Follow/unfollow operations should be idempotent.
* Persist mutations safely.
* No authentication, external APIs, or database server.

