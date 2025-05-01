---
title: Suggestions API
description: API endpoint for fetching product suggestions.
---

Provides endpoints for retrieving suggested questions related to products.

## Get Suggested Questions

Fetches a list of suggested questions associated with a given product ID.

**Endpoint:** `GET https://api.productgenie.io/suggestions`

**Authentication:** Requires website public key (`X-Public-Key` header).

### Query Parameters

| Parameter  | Type   | Required | Description                                                          | Example     |
| :--------- | :----- | :------- | :------------------------------------------------------------------- | :---------- |
| product_id | string | Yes      | The unique identifier of the product for which to fetch suggestions. | `prod_123`  |
| website_id | string | No       | The identifier of the website (optional, used for lookup).         | `website_abc` |

### Responses

-   **200 OK:** Suggested questions retrieved successfully.
    ```json
    {
      "id": "prod_123",
      "name": "Super Widget",
      "suggested_questions": [
        "What colors does it come in?",
        "Is it easy to assemble?"
      ]
    }
    ```
-   **400 Bad Request:** Missing `product_id`.
-   **401 Unauthorized:** Missing or invalid `X-Public-Key`.
-   **404 Not Found:** `product_id` does not exist for the given website.
-   **500 Internal Server Error:** Error accessing the database. 