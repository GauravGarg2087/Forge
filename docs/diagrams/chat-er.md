# Chat Service – ER Diagram

```mermaid
erDiagram
    CONVERSATION ||--o{ CONVERSATION_MEMBER : contains
    CONVERSATION ||--o{ MESSAGE : has

    CONVERSATION {
        UUID conversation_id PK
        string conversation_type
        UUID project_id
        string name
        UUID created_by_member_id
        datetime created_at
    }

    CONVERSATION_MEMBER {
        UUID conversation_id PK, FK
        UUID member_id PK
        datetime joined_at
    }

    MESSAGE {
        UUID message_id PK
        UUID conversation_id FK
        UUID sender_member_id
        string content
        datetime created_at
        datetime updated_at
    }

    MEMBER_CACHE {
        UUID member_id PK
        string name
        string avatar_url
        datetime updated_at
    }
```

A conversation can represent either a project channel or a direct conversation.

Member IDs and project IDs refer to information owned by other services. The Chat Service does not maintain its own Member or Project tables.
