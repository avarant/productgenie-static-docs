---
title: Reviews API
description: API endpoints for managing product reviews.
---

Provides endpoints for creating, updating, and deleting product reviews.

**Authentication:** Requires API key (`X-Public-Key`) and HMAC signature (`X-Productgenie-Signature`) for all operations.

## Create Review

Creates a new product review or replaces an existing one with the same `review_id`. Generates text embeddings for the review content.

**Endpoint:** `POST https://api.productgenie.io/review`

### Request Body

Requires a JSON payload with the following structure (`ReviewPayload`):

```json
{
  "review_id": "review_789",
  "product_id": "prod_123",
  "review_author": "John Smith",
  "review_content": "This product worked great!",
  "product_name": "Super Widget",
  "website_name": "Example Store", // Optional
  "website_url": "https://example.com", // Optional
  "rating": 5 // Optional
}
```

### Responses

-   **200 OK:** Review created/processed successfully. (`SuccessResponse`)
-   **400 Bad Request:** Missing fields, invalid JSON.
-   **401 Unauthorized:** Invalid signature.
-   **500 Internal Server Error.**

## Update Review

Updates an existing product review identified by `review_id`. Regenerates text embeddings.

**Endpoint:** `PUT https://api.productgenie.io/review`

### Request Body

Requires a JSON payload with the `ReviewPayload` structure (see Create Review).

### Responses

-   **200 OK:** Review updated successfully. (`SuccessResponse`)
-   **400 Bad Request:** Missing fields, invalid JSON.
-   **401 Unauthorized:** Invalid signature.
-   **404 Not Found:** `review_id` does not exist.
-   **500 Internal Server Error.**

## Delete Review

Deletes an existing product review identified by `review_id`.

**Endpoint:** `DELETE https://api.productgenie.io/review`

### Request Body

Requires a JSON payload with the `review_id`:

```json
{
  "review_id": "review_789"
}
```

### Responses

-   **200 OK:** Review deleted successfully. (`SuccessResponse`)
-   **400 Bad Request:** Missing `review_id`, invalid JSON.
-   **401 Unauthorized:** Invalid signature.
-   **404 Not Found:** `review_id` does not exist.
-   **500 Internal Server Error.** 