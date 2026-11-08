\# ERD Khwarizmi WorkOS



\## Diagram Hubungan



```mermaid

erDiagram

&#x20;   CLIENT ||--o{ USER : "punya"

&#x20;   CLIENT ||--o{ PROJECT : "memiliki"

&#x20;   USER ||--o{ PROJECT : "membuat"

&#x20;   USER ||--o{ PROJECT\_MEMBER : "ikut"

&#x20;   PROJECT ||--o{ PROJECT\_MEMBER : "punya anggota"

&#x20;   PROJECT ||--o{ MILESTONE : "punya"

&#x20;   PROJECT ||--o{ TASK : "punya"

&#x20;   MILESTONE |o--o{ TASK : "mengelompokkan"

&#x20;   USER ||--o{ TASK : "dikerjakan (assignee)"

&#x20;   TASK ||--o{ COMMENT : "punya"

&#x20;   USER ||--o{ COMMENT : "menulis"

&#x20;   TASK ||--o{ APPROVAL : "punya"

&#x20;   USER ||--o{ APPROVAL : "memutuskan"

&#x20;   USER ||--o{ ACTIVITY\_LOG : "pelaku"

&#x20;   USER ||--o{ NOTIFICATION : "menerima"

```



\## Kolom Tiap Tabel



\- \*\*User\*\*: id, name, email, passwordHash, role, clientId (kosong kalau bukan Client)

\- \*\*Client\*\*: id, name

\- \*\*Project\*\*: id, name, clientId, deadline, createdBy

\- \*\*ProjectMember\*\*: userId, projectId, joinedAt

\- \*\*Milestone\*\*: id, projectId, title, dueDate

\- \*\*Task\*\*: id, projectId, milestoneId (boleh kosong), title, description, status, priority, assigneeId, dueDate, version

\- \*\*Comment\*\*: id, taskId, userId, body, createdAt

\- \*\*Approval\*\*: id, taskId, clientUserId, decision, reason, createdAt

\- \*\*ActivityLog\*\*: id, actorId, action, entityType, entityId, before, after, createdAt

\- \*\*Notification\*\*: id, userId, message, isRead, createdAt



\## Catatan Desain



\- Hak akses selalu dicek dengan "naik" ke Project, lalu memeriksa ProjectMember (atau clientId untuk Client).

\- Approval dan ActivityLog hanya boleh ditambah barisnya, tidak boleh diubah atau dihapus.

\- Kolom `version` di Task dipakai untuk mencegah update yang bentrok (tantangan A2).

