# Forge – Project Proposal

## 1. Introduction

Forge is a task tracker for projects, built as polyglot microservices over gRPC and events, with Claude reviewing pull requests and real-time mentor-developer chat.

## 2. Features

### User Management
- Channel i authentication.
- Email/password authentication with OTP as an alternative.
- Member profiles and role-based permissions.

### Project Management
- Create and manage projects.
- Add and manage project members.
- Organise work using milestones.

### Task Management
- Create, assign, edit and delete tasks.
- Set priorities and deadlines.
- Add descriptions and acceptance criteria.
- Add labels, subtasks and dependencies.
- Support task comments and handoffs.
- Maintain an activity timeline.

Task statuses:
- Backlog
- Todo
- In Progress
- In Review
- Blocked
- Done
- Cancelled

Task priorities:
- Urgent
- High
- Medium
- Low

### Team Communication
- Project chat channels.
- Task discussions.
- Direct messages.
- Real-time messaging and member presence.

### GitHub Integration
- Link pull requests to tasks.
- Track pull request status.
- Display CI results.
- Integrate Claude-powered code reviews.
- Allow mentors to review and approve completed work.

### MCP Integration
Allow mentors to interact with Forge through AI assistants.

Available operations will include:
- Listing projects and tasks.
- Searching and viewing tasks.
- Creating and updating tasks.
- Assigning tasks.
- Adding comments and handing off tasks.

### AI Agents
- Triage Agent: Suggests task categories and priorities.
- Review Agent: Reviews code against task requirements.
- Standup Agent: Generates project progress summaries.
- Nudge Agent: Identifies overdue and blocked tasks.

Agent activities will be recorded.

## 3. Proposed Architecture

Forge will use four independent microservices:

1. Identity Service – Manages authentication, members and roles.
2. Tracker Service – Manages projects, milestones and tasks.
3. Chat Service – Handles real-time communication.
4. AI/MCP Service – Provides MCP tools and AI agents.

A frontend and API Gateway will connect users to these services.

Each stateful service will maintain its own database. Services will communicate using gRPC and events.

## 4. Proposed Technologies

- Frontend: React, HTML, CSS and JavaScript.
- Backend: Python, JavaScript/TypeScript, Go and Java.
- Database: PostgreSQL.
- Communication: gRPC, Protocol Buffers and message queues.
- Real-time communication: WebSockets.
- Deployment: Docker and Docker Compose.
- Version control: Git and GitHub.

Technology choices may be refined during development.

## 5. Development Plan

1. Finalise architecture, database design and service contracts.
2. Develop Identity and Tracker services.
3. Implement the API Gateway and MCP integration.
4. Develop Chat and the frontend.
5. Integrate GitHub, Claude reviews and AI agents.
6. Test and deploy the complete application.

## 6. Future Enhancements

- Email notifications and deadline reminders.
- Analytics dashboard.
- Semantic search.
- Converting chat messages into tasks.
- Additional microservices if required.

## 7. Expected Outcome

The goal is to build a functional and modular platform that simplifies project management and collaboration while demonstrating practical applications of microservices, distributed communication, GitHub integration and AI tools.
