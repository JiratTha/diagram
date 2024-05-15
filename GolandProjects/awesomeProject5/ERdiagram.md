```mermaid
erDiagram
    USER {
        int user_id PK
        string username
        string email
        string password
    }

    POINTS {
        int points_id PK
        int user_id FK
        int total_points
    }

    POINT_TRANSACTION {
        int transaction_id PK
        int sender_id FK
        int receiver_id FK
        int points_transferred
        datetime transaction_date
    }

    USER ||--o{ POINTS: has
    USER ||--o{ POINT_TRANSACTION: sends
    USER ||--o{ POINT_TRANSACTION: receives
    POINTS ||--o| USER: belongs_to
    POINT_TRANSACTION }o--|| USER: to
```