# Forge – MCP Tools

The AI/MCP Service exposes tools that allow AI assistants to interact with Forge.

These tools operate through the APIs of other services rather than accessing their databases directly.

## 1. Project Tools

### list_projects

Lists projects accessible to the authenticated member.

**Input:**
- member_id (optional)

**Output:**
- project_id
- name
- description
- project_role

## 2. Task Tools

### list_tasks

Lists tasks in a project.

**Input:**
- project_id
- status (optional)
- assignee_member_id (optional)

**Output:**
- List of matching tasks.

### my_tasks

Lists tasks assigned to the authenticated member.

**Input:**
- status (optional)

**Output:**
- List of assigned tasks.

### get_task

Returns details of a particular task.

**Input:**
- task_id

**Output:**
- Task details.
- Assignee information.
- Status and priority.
- Comments and activity history.

### search_tasks

Searches tasks using a text query and optional filters.

**Input:**
- query
- project_id (optional)

**Output:**
- List of matching tasks.

### create_task

Creates a new task.

**Input:**
- project_id
- title
- description
- priority
- assignee_member_id (optional)
- due_date (optional)

**Output:**
- Created task and its task_id.

### update_task

Updates an existing task.

**Input:**
- task_id
- fields to update

**Output:**
- Updated task details.

### assign_task

Assigns a task to a member.

**Input:**
- task_id
- member_id

**Output:**
- Updated assignment information.

## 3. Collaboration Tools

### add_comment

Adds a comment to a task.

**Input:**
- task_id
- content

**Output:**
- Created comment.

### handoff_task

Transfers task responsibility to another member.

**Input:**
- task_id
- to_member_id
- message (optional)

**Output:**
- Handoff details.

## 4. AI Agents

### Triage Agent

Suggests task categories, priorities and assignments based on task descriptions.

### Review Agent

Reviews code changes against task requirements and produces review feedback.

### Standup Agent

Generates project progress summaries using task activity.

### Nudge Agent

Identifies overdue and blocked tasks and suggests follow-up actions.

## 5. Security

- MCP requests must be authenticated.
- Personal access tokens should have limited scopes.
- Tokens should expire and be revocable.
- Tools must check permissions before performing write operations.
- Tool calls and agent activities should be recorded.

