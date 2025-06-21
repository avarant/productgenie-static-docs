---
title: Products API
description: API endpoints for managing products in ProductGenie.
---

The Products API provides endpoints for managing product information, including creating, updating, deleting, and retrieving product details. These endpoints are typically used by e-commerce platforms to sync their product catalog with ProductGenie.

**Base URL:** `https://api.productgenie.io`

**Authentication:** Write operations (POST, PUT, DELETE) require API key (`X-Public-Key`) and HMAC signature (`X-Productgenie-Signature`). Read operations require only the API key.

## Get Product Details

Retrieves details for a specific product, including suggested questions.

**Endpoint:** `GET /product`

### Query Parameters

| Parameter   | Type   | Required | Description                                      | Example             |
| :---------- | :----- | :------- | :----------------------------------------------- | :------------------ |
| product_id  | string | Yes      | The ID of the product to retrieve               | `prod_123`          |
| website_id  | string | Yes      | The ID of the website associated with the product | `web_456`          |

### Response

**200 OK** - Product details retrieved successfully

```json
{
  "id": "prod_123",
  "name": "Super Widget",
  "suggested_questions": [
    "What are the dimensions?",
    "Is it compatible with...",
    "What is the warranty?"
  ]
}
```

**404 Not Found** - Product or website not found

```json
{
  "error": "Product not found",
  "details": "The specified product does not exist"
}
```

## Create Products

Creates one or more products for a website. This endpoint requires signature authentication.

**Endpoint:** `POST /product`

### Request Body

```json
{
  "products": [
    {
      "id": "prod_123",
      "name": "Super Widget",
      "description": "The best widget ever made"
    },
    {
      "id": "prod_456",
      "name": "Mega Gadget",
      "description": "A revolutionary gadget for modern living"
    }
  ]
}
```

### Response

**200 OK** - Products created successfully

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

## Update Products

Updates one or more existing products. This endpoint requires signature authentication.

**Endpoint:** `PUT /product`

### Request Body

Same structure as the POST endpoint:

```json
{
  "products": [
    {
      "id": "prod_123",
      "name": "Super Widget v2",
      "description": "The improved widget with new features"
    }
  ]
}
```

### Response

**200 OK** - Products updated successfully

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

## Delete Products

Deletes one or more products from a website. This endpoint requires signature authentication.

**Endpoint:** `DELETE /product`

### Request Body

```json
{
  "products": [
    {
      "id": "prod_123"
    },
    {
      "id": "prod_456"
    }
  ]
}
```

### Response

**200 OK** - Products deleted successfully

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

## Authentication

### Read Operations (GET)

Requires only the API key in the header:

```
X-Public-Key: your-api-key
```

### Write Operations (POST, PUT, DELETE)

Requires both API key and HMAC signature:

```
X-Public-Key: your-api-key
X-Productgenie-Signature: your-hmac-signature
```

The signature should be computed using HMAC-SHA256 with your secret key and the request body. 