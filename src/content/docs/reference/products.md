---
title: Products API
description: API endpoints for managing products.
---

Provides endpoints for creating, updating, deleting, and retrieving product information.

**Authentication:** Requires API key (`X-Public-Key`) and HMAC signature (`X-Productgenie-Signature`) for all operations.

## Create or Update Products

Creates new products or updates existing ones based on the provided data.

**Endpoint:** `POST https://api.productgenie.io/product`

### Request Body

Requires a JSON payload:

```json
{
  "website_url": "https://example.com",
  "products": [
    {
      "id": "prod_123", // Required for updates
      "name": "Super Widget",
      "description": "The best widget ever."
    }
    // ... more products
  ]
}
```

### Responses

-   **200 OK:** Products processed successfully.
    ```json
    {
      "message": "Products processed successfully.",
      "processed_count": 1
    }
    ```
-   **400 Bad Request:** Missing fields, invalid data.
-   **401 Unauthorized:** Invalid signature.
-   **500 Internal Server Error.**

## Delete Products

Deletes products based on the provided IDs.

**Endpoint:** `DELETE https://api.productgenie.io/product`

### Request Body

Requires a JSON payload:

```json
{
  "website_url": "https://example.com",
  "product_ids": [
    "prod_123",
    "prod_456"
  ]
}
```

### Responses

-   **200 OK:** Products deleted successfully.
    ```json
    {
      "message": "Products deleted successfully.",
      "deleted_count": 2
    }
    ```
-   **400 Bad Request:** Missing fields, invalid IDs.
-   **401 Unauthorized:** Invalid signature.
-   **500 Internal Server Error.**

## Get Product Details

Retrieves details for a specific product.

**Endpoint:** `GET https://api.productgenie.io/product`

### Query Parameters

| Parameter   | Type   | Required | Description                                      | Example             |
| :---------- | :----- | :------- | :----------------------------------------------- | :------------------ |
| product_id  | string | Yes      | The ID of the product to retrieve.               | `prod_123`          |
| website_url | string | Yes      | The URL of the website associated with the product. | `https://example.com` |

### Responses

-   **200 OK:** Product details retrieved successfully.
    ```json
    {
      "id": "prod_123",
      "name": "Super Widget",
      "description": "The best widget ever."
    }
    ```
-   **400 Bad Request:** Missing `product_id`.
-   **401 Unauthorized:** Invalid signature.
-   **404 Not Found:** Product ID does not exist.
-   **500 Internal Server Error.** 