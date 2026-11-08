# Forge – Database Design

Forge uses separate PostgreSQL databases for its Identity, Tracker and Chat services. Each service owns its database and manages its data independently.

## 1. Identity Service Database

The Identity Service manages member accounts, authentication and roles.

### Member
Stores basic member information.

- member_id (Primary Key)
- name
- email (Unique)
- avatar_url
- created_at
- updated_at

### AuthAccount
Stores authentication methods linked to a member.

- auth_id (Primary Key)
- member_id (Foreign Key)
- provider (CHANNEL_I / EMAIL)
- provider_user_id
- password_hash (Nullable)
- created_at

### Role
Stores the roles available in the system.

- role_id (Primary Key)
- role_name (Unique)
- description

### MemberRole
Connects members with their assigned roles.

- member_id (Foreign Key)
- role_id (Foreign Key)
- assigned_at

Primary Key: (member_id, role_id)

A member can have multiple roles, and a role can be assigned to multiple members.

## 2. Tracker Service Database

The Tracker Service manages projects, tasks and their associated activities.

### Project
- project_id (Primary Key)
- name
- description
- created_by (Member ID)
- created_at
- updated_at

### ProjectMember
Stores members associated with each project.

- project_id (Foreign Key)
- member_id (Member ID)
- project_role
- joined_at

Primary Key: (project_id, member_id)

### Milestone
- milestone_id (Primary Key)
- project_id (Foreign Key)
- name
- description
- due_date
- status

### Task
- task_id (Primary Key)
- project_id (Foreign Key)
- milestone_id (Foreign Key, Nullable)
- parent_task_id (Self-referencing Foreign Key, Nullable)
- title
- description
- task_type
- status
- priority
- assignee_member_id (Member ID, Nullable)
- reporter_member_id (Member ID)
- due_date
- created_at
- updated_at

The parent_task_id field allows tasks to have subtasks.

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

### Comment
- comment_id (Primary Key)
- task_id (Foreign Key)
- author_member_id (Member ID)
- content
- created_at
- updated_at

### Label
- label_id (Primary Key)
- project_id (Foreign Key)
- name

### TaskLabel
Connects tasks with their labels.

- task_id (Foreign Key)
- label_id (Foreign Key)

Primary Key: (task_id, label_id)

### TaskDependency
Stores dependencies between tasks.

- task_id (Foreign Key)
- depends_on_task_id (Foreign Key)
- dependency_type

Primary Key: (task_id, depends_on_task_id)

### TaskActivity
Maintains the activity timeline of a task.

- activity_id (Primary Key)
- task_id (Foreign Key)
- actor_member_id (Member ID)
- activity_type
- details
- created_at

### TaskHandoff
Records the transfer of responsibility between members.

- handoff_id (Primary Key)
- task_id (Foreign Key)
- from_member_id (Member ID)
- to_member_id (Member ID)
- message
- created_at

### MemberCache
Stores basic member information received from Identity.

- member_id (Primary Key)
- name
- avatar_url
- updated_at

## 3. Chat Service Database

The Chat Service manages conversations, channels and messages.

### Conversation
Represents either a project channel or a direct conversation.

- conversation_id (Primary Key)
- conversation_type (PROJECT / DIRECT)
- project_id (Nullable, external reference)
- name (Nullable)
- created_by_member_id (Member ID)
- created_at

### ConversationMember
Stores members participating in a conversation.

- conversation_id (Foreign Key)
- member_id (Member ID)
- joined_at

Primary Key: (conversation_id, member_id)

### Message
Stores messages sent in a conversation.

- message_id (Primary Key)
- conversation_id (Foreign Key)
- sender_member_id (Member ID)
- content
- created_at
- updated_at

### MemberCache
Stores basic member information received from Identity.

- member_id (Primary Key)
- name
- avatar_url
- updated_at

## 4. Data Ownership

- Identity owns all member accounts and role information.
- Tracker stores member IDs and maintains a local cache of basic member information.
- Chat also stores member IDs and maintains a local member cache.
- Project IDs referenced by Chat belong to Tracker.
- Services never directly access another service's database.
- Shared information is exchanged through APIs, contracts and events.
- The AI/MCP Service does not own core domain data and does not require a separate domain database.
