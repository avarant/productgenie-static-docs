---
title: Message API
description: API endpoints for managing conversations and messages with the AI assistant.
---

The Message API provides endpoints for managing conversations with ProductGenie's AI assistant. It supports creating conversations, sending messages, retrieving conversation history, voting on responses, and clearing sessions.

**Base URL:** `https://api.productgenie.io`

**Authentication:** Requires API key (`X-Public-Key`) header for all operations.

**Session Management:** Uses HTTP cookies (`conversation_session`) to maintain conversation state across requests.

## Get Conversation History

Retrieves the message history for the current user's session.

**Endpoint:** `GET /message`

### Response

**200 OK** - Array of messages in the conversation

```json
[
  {
    "ai_message": "Hello! I'm here to help you with product questions.",
    "conversation_id": "550e8400-e29b-41d4-a716-446655440000",
    "message_id": "660e8400-e29b-41d4-a716-446655440001",
    "products": []
  },
  {
    "ai_message": "The Super Widget comes in three colors: red, blue, and green.",
    "conversation_id": "550e8400-e29b-41d4-a716-446655440000",
    "message_id": "770e8400-e29b-41d4-a716-446655440002",
    "products": [
      {
        "id": "prod_123",
        "name": "Super Widget"
      }
    ]
  }
]
```

**401 Unauthorized** - Invalid public key

```json
{
  "error": "Unauthorized",
  "details": "Invalid or missing API key"
}
```

## Send a Message

Handles one turn in a conversation. Takes a user message, gets an AI response, and stores both.

**Endpoint:** `POST /message`

### Request Body

```json
{
  "message": "What colors does the Super Widget come in?",
  "conversation_id": "550e8400-e29b-41d4-a716-446655440000",
  "product_id": "prod_123"
}
```

### Request Fields

| Field           | Type   | Required | Description                                                                           |
| :-------------- | :----- | :------- | :------------------------------------------------------------------------------------ |
| message         | string | Yes      | The user's message content                                                            |
| conversation_id | string | No       | UUID of existing conversation. If omitted, uses cookie or creates new conversation   |
| product_id      | string | No       | ID of the product this message is about                                              |

### Response

**200 OK** - Successful response from the AI

```json
{
  "ai_message": "The Super Widget is available in three colors: red, blue, and green. Each color has the same features and specifications.",
  "conversation_id": "550e8400-e29b-41d4-a716-446655440000",
  "message_id": "770e8400-e29b-41d4-a716-446655440002",
  "products": [
    {
      "id": "prod_123",
      "name": "Super Widget",
      "price": "$49.99"
    }
  ]
}
```

### Response Headers

When a new conversation is created, the response includes:

```
Set-Cookie: conversation_session=550e8400-e29b-41d4-a716-446655440000; Path=/; HttpOnly; Secure; SameSite=Strict
```

**400 Bad Request** - Missing required fields

```json
{
  "error": "Bad Request",
  "details": "Missing required field: message"
}
```

**401 Unauthorized** - Invalid public key

```json
{
  "error": "Unauthorized",
  "details": "Invalid or missing API key"
}
```

## Vote on a Message

Upvote or downvote a specific AI message to provide feedback.

**Endpoint:** `POST /message/{message_id}/vote`

### Path Parameters

| Parameter  | Type   | Required | Description                    |
| :--------- | :----- | :------- | :----------------------------- |
| message_id | string | Yes      | UUID of the message to vote on |

### Request Body

```json
{
  "action": "upvote"
}
```

### Request Fields

| Field  | Type   | Required | Description                          |
| :----- | :----- | :------- | :----------------------------------- |
| action | string | Yes      | Vote action: "upvote" or "downvote" |

### Response

**200 OK** - Vote recorded successfully

```json
{
  "message": "Vote recorded successfully",
  "upvotes": 5,
  "downvotes": 1
}
```

**400 Bad Request** - Invalid action

```json
{
  "error": "Bad Request",
  "details": "Invalid action. Must be 'upvote' or 'downvote'"
}
```

**403 Forbidden** - Message does not belong to website

```json
{
  "error": "Forbidden",
  "details": "Message does not belong to your website"
}
```

**404 Not Found** - Message not found

```json
{
  "error": "Not Found",
  "details": "Message not found"
}
```

## Clear Conversation Session

Clears the user's current conversation session by expiring the session cookie.

**Endpoint:** `POST /message/clear`

### Response

**200 OK** - Session cleared successfully

```json
{
  "status": "ok"
}
```

### Response Headers

```
Set-Cookie: conversation_session=; Path=/; Expires=Thu, 01 Jan 1970 00:00:00 GMT
```

**401 Unauthorized** - Invalid public key

```json
{
  "error": "Unauthorized",
  "details": "Invalid or missing API key"
}
```

## CORS Support

All message endpoints support CORS (Cross-Origin Resource Sharing) with OPTIONS methods for preflight requests.

## Session Management

- Sessions are managed via the `conversation_session` cookie
- If no cookie is present, a new conversation is created automatically
- Sessions persist across multiple requests until explicitly cleared
- The cookie is HTTP-only, secure, and uses strict same-site policy