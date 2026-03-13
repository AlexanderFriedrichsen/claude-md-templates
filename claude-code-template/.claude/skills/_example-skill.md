---
name: example-domain
description: Example domain reference — consult before writing any [domain area] logic. DELETE THIS FILE and create your own skills.
---

# [Domain Name] Reference

## Overview

<!-- 1-2 sentences describing what this skill covers and when to consult it. -->

## Data Model

<!-- Define the core types/interfaces that this domain uses. -->

```typescript
interface ExampleEntity {
  id: string;
  name: string;
  // ...
}
```

## Business Rules

<!-- List the rules that code in this domain must follow. -->

- Rule 1: [description]
- Rule 2: [description]

## API Reference (if applicable)

<!-- If this domain involves an external API, document the key endpoints. -->

### Base URL

`https://api.example.com/v1`

### Key Endpoints

- `GET /items` — List items (paginated)
- `POST /items` — Create item
- `GET /items/{id}` — Get single item

### Authentication

- Header: `Authorization: Bearer <token>`
- Env var: `EXAMPLE_API_KEY`

### Rate Limits

- [e.g., 100 requests/minute, 1000/hour]

## Common Gotchas

<!-- Things that have tripped you up or are easy to get wrong. -->

- [Gotcha 1]
- [Gotcha 2]

## What NOT to Do in Code

- [Anti-pattern 1]
- [Anti-pattern 2]

## Testing Notes

- Always mock [external service] in tests
- Test [specific edge case] because [reason]
