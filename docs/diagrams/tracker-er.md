# Tracker Service – ER Diagram

```mermaid
erDiagram
    PROJECT ||--o{ PROJECT_MEMBER : has
    PROJECT ||--o{ MILESTONE : contains
    PROJECT ||--o{ TASK : contains
    PROJECT ||--o{ LABEL : defines

    MILESTONE o|--o{ TASK : groups
    TASK o|--o{ TASK : parent_of

    TASK ||--o{ COMMENT : has
    TASK ||--o{ TASK_ACTIVITY : records
    TASK ||--o{ TASK_HANDOFF : has

    TASK ||--o{ TASK_LABEL : tagged
    LABEL ||--o{ TASK_LABEL : assigned

    TASK ||--o{ TASK_DEPENDENCY : has
    TASK ||--o{ TASK_DEPENDENCY : referenced_by

    PROJECT {
        UUID project_id PK
        string name
        string description
        UUID created_by
        datetime created_at
        datetime updated_at
    }

    PROJECT_MEMBER {
        UUID project_id PK, FK
        UUID member_id PK
        string project_role
        datetime joined_at
    }

    MILESTONE {
        UUID milestone_id PK
        UUID project_id FK
        string name
        string description
        date due_date
        string status
    }

    TASK {
        UUID task_id PK
        UUID project_id FK
        UUID milestone_id FK
        UUID parent_task_id FK
        string title
        string description
        string task_type
        string status
        string priority
        UUID assignee_member_id
        UUID reporter_member_id
        date due_date
        datetime created_at
        datetime updated_at
    }

    COMMENT {
        UUID comment_id PK
        UUID task_id FK
        UUID author_member_id
        string content
        datetime created_at
        datetime updated_at
    }

    LABEL {
        UUID label_id PK
        UUID project_id FK
        string name
    }

    TASK_LABEL {
        UUID task_id PK, FK
        UUID label_id PK, FK
    }

    TASK_DEPENDENCY {
        UUID task_id PK, FK
        UUID depends_on_task_id PK, FK
        string dependency_type
    }

    TASK_ACTIVITY {
        UUID activity_id PK
        UUID task_id FK
        UUID actor_member_id
        string activity_type
        string details
        datetime created_at
    }

    TASK_HANDOFF {
        UUID handoff_id PK
        UUID task_id FK
        UUID from_member_id
        UUID to_member_id
        string message
        datetime created_at
    }

    MEMBER_CACHE {
        UUID member_id PK
        string name
        string avatar_url
        datetime updated_at
    }
```

Member IDs refer to members owned by the Identity Service. They are not foreign keys to another service's database.

Both task_id and depends_on_task_id in TASK_DEPENDENCY refer to records in the Tracker Service's own TASK table.
