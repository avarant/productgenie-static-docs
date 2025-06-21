---
title: Reviews API
description: API endpoints for managing product reviews in ProductGenie.
---

The Reviews API provides endpoints for managing product reviews, including creating, updating, and deleting reviews. These endpoints enable e-commerce platforms to sync their customer reviews with ProductGenie for enhanced product insights.

**Base URL:** `https://api.productgenie.io`

**Authentication:** All review operations require both API key (`X-Public-Key`) and HMAC signature (`X-Productgenie-Signature`) headers.

## Create Review

Creates a new product review. This endpoint requires signature authentication.

**Endpoint:** `POST /review`

### Request Body

```json
{
  "review_id": "review_789",
  "review_content": "This product exceeded my expectations! Great quality.",
  "product_id": "prod_123",
  "rating": 5
}
```

### Request Fields

| Field           | Type    | Required | Description                               |
| :-------------- | :------ | :------- | :---------------------------------------- |
| review_id       | string  | Yes      | Unique identifier for the review          |
| review_content  | string  | No       | The text content of the review            |
| product_id      | string  | No       | ID of the product being reviewed          |
| rating          | integer | No       | Rating value (typically 1-5)              |

### Response

**200 OK** - Review created successfully

```json
{}
```

**401 Unauthorized** - Invalid signature or public key

```json
{
  "error": "Unauthorized",
  "details": "Invalid signature or API key"
}
```

## Update Review

Updates an existing product review. This endpoint requires signature authentication.

**Endpoint:** `PUT /review`

### Request Body

Same structure as the POST endpoint:

```json
{
  "review_id": "review_789",
  "review_content": "Updated: This product is even better after extended use!",
  "product_id": "prod_123",
  "rating": 5
}
```

### Response

**200 OK** - Review updated successfully

```json
{}
```

**401 Unauthorized** - Invalid signature or public key

```json
{
  "error": "Unauthorized",
  "details": "Invalid signature or API key"
}
```

**404 Not Found** - Review not found

```json
{
  "error": "Review not found",
  "details": "The specified review does not exist"
}
```

## Delete Review

Deletes an existing product review. This endpoint requires signature authentication.

**Endpoint:** `DELETE /review`

### Request Body

```json
{
  "review_id": "review_789"
}
```

### Response

**200 OK** - Review deleted successfully

```json
{}
```

**401 Unauthorized** - Invalid signature or public key

```json
{
  "error": "Unauthorized",
  "details": "Invalid signature or API key"
}
```

**404 Not Found** - Review not found

```json
{
  "error": "Review not found",
  "details": "The specified review does not exist"
}
```

## Authentication Headers

All review operations require both headers:

```
X-Public-Key: your-api-key
X-Productgenie-Signature: your-hmac-signature
```

The signature should be computed using HMAC-SHA256 with your secret key and the request body. 