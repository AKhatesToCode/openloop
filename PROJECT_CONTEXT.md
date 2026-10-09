# OPENLOOP — Project Context

## Product

OPENLOOP is a social discovery application where live conversations create persistent connection trails. Users discover rooms, join discussions, save takeaways and questions, follow topics, and create linked follow-up rooms.

## Technology Stack

* Frontend: React, Vite, TypeScript
* Backend: Node.js, Express, TypeScript
* Persistence: Local JSON file (`server/data/db.json`)
* Development: GitHub, Claude, Ollama Qwen3.5:4B, Antigravity

## Constraints

* No authentication or Supabase.
* No paid APIs or external services required.
* Core demo must work without audio or an LLM.
* One local backend process.
* Minimal dependencies and simple, explainable code.
* Frontend and backend must follow `API.md`.
* Do not invent endpoints or data fields.
* Test changes before claiming they work.

## MVP Features

1. Discover live and ended rooms.
2. Filter rooms by topic.
3. View room details and participants.
4. Join, leave, and end rooms.
5. Save takeaways and questions to room trails.
6. Create follow-up rooms linked to parent rooms.
7. Follow and unfollow topics.
8. View a user's saved trail items.

## Demo Behaviour

* Seed sample users, topics, and rooms.
* Use a seeded demo user as the active user.
* No login flow.
* Persist changes to the local JSON database.
* Reloading the application must preserve saved changes.

## Architecture Rules

* Keep modules small and responsibilities clear.
* Keep API contracts and shared types consistent.
* Validate inputs and handle errors.
* Avoid unnecessary abstractions and features.
* Update `README.md` whenever implementation or run instructions change.

## Implementation Order

1. Agree on data types and API contract.
2. Implement and test backend persistence and routes.
3. Implement frontend against the agreed API.
4. Integrate and test the complete demo.
5. Update documentation to match the actual code.
