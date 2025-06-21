---
title: Suggestions API
description: API endpoint for retrieving suggested questions for products.
---

The Suggestions API provides an endpoint for retrieving AI-generated suggested questions for products. These questions help guide customer conversations and improve the shopping experience.

**Base URL:** `https://api.productgenie.io`

**Authentication:** Requires API key (`X-Public-Key`) header for all operations.

## Get Suggested Questions

Retrieves a list of suggested questions for a specific product.

**Endpoint:** `GET /suggestions/questions`

### Query Parameters

| Parameter  | Type   | Required | Description                                                          | Example       |
| :--------- | :----- | :------- | :------------------------------------------------------------------- | :------------ |
| product_id | string | Yes      | The unique identifier of the product for which to fetch suggestions | `prod_123`    |
| website_id | string | No       | The identifier of the website (optional, used for lookup)          | `web_456`     |

### Response

**200 OK** - Suggested questions retrieved successfully

```json
{
  "id": "prod_123",
  "name": "Super Widget",
  "suggested_questions": [
    "What are the dimensions of this product?",
    "Is assembly required?",
    "What materials is it made from?",
    "Does it come with a warranty?",
    "What colors are available?"
  ]
}
```

**401 Unauthorized** - Invalid public key

```json
{
  "error": "Unauthorized",
  "details": "Invalid or missing API key"
}
```

**404 Not Found** - Product not found

```json
{
  "error": "Product not found",
  "details": "The specified product does not exist"
}
```

## CORS Support

This endpoint supports CORS (Cross-Origin Resource Sharing) with an OPTIONS method for preflight requests.

## Authentication Header

```
X-Public-Key: your-api-key
``` 