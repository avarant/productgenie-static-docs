---
title: Questions API
description: API endpoint for asking questions about products.
---

Provides an endpoint for customers to ask questions about products and receive AI-generated answers based on existing reviews.

## Ask a Product Question

Takes a customer's question about a specific product, finds relevant reviews using vector search, and generates an answer using an LLM.

**Endpoint:** `POST https://api.productgenie.io/question`

**Authentication:** Requires website public key (`X-Public-Key` header).

### Request Body

Requires a JSON payload:

```json
{
  "question": "Is this product good for sensitive skin?",
  "product_id": "prod_123",
  "website_url": "https://example.com"
}
```

### Responses

-   **200 OK:** Answer generated successfully.
    ```json
    {
      "answer": "Based on customer reviews, this product is generally considered suitable for sensitive skin, but individual experiences may vary.",
      "question_id": "q_abcdef123456"
    }
    ```
-   **400 Bad Request:** Missing fields, product not found, question limit reached, no relevant reviews found.
-   **401 Unauthorized:** Missing or invalid `X-Public-Key`.
-   **500 Internal Server Error:** Error during embedding, vector search, AI generation, or database write. 