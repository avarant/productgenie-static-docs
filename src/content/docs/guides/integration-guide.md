---
title: "Integration Guide"
description: "Learn how to integrate the Product Genie widget using Vanilla JavaScript and iframes."
---

## Vanilla JavaScript Integration

Embed the Product Genie widget using a simple HTML snippet and our provided embed script.

**Steps:**

1.  **Obtain Keys:** Get your unique `Public Key` from your Product Genie dashboard.

2.  **Add Container Div:** Place the following `div` element in your HTML where you want the widget to appear. Replace the placeholder values with your actual `Public Key` and the `Product ID` for the specific product page.

    ```html
    <div 
        id="productgenie-widget"
        data-public-key="YOUR_PUBLIC_KEY"
        data-product-id="YOUR_PRODUCT_ID">
    </div>
    ```

    *   Replace `YOUR_PUBLIC_KEY` with the key obtained in step 1.
    *   Replace `YOUR_PRODUCT_ID` with the specific ID for this product (this could be a SKU, database ID, etc., consistent with how you identify products in Product Genie).

3.  **Include Embed Script:** Add the following script tag to your HTML page, preferably just before the closing `</body>` tag. 

    ```html
    <script src="https://widget.productgenie.io/embed.js" async defer></script>
    ```

    *   Make sure the `src` attribute points to the location where the `embed.js` file is hosted. The URL `https://widget.productgenie.io/embed.js` is the default location.
    *   The `async` and `defer` attributes ensure the script loads without blocking your page rendering.


**How it works:**

The `embed.js` script will automatically:
*   Find the `<div id="productgenie-widget">`.
*   Read the `data-public-key` and `data-product-id` attributes.
*   Determine the current page URL (`website_url`).
*   Create an `iframe` pointing to the Product Genie widget application, passing the necessary configuration as URL parameters.
*   Insert the `iframe` into the container div.
*   Listen for messages from the iframe to automatically adjust its height, ensuring seamless integration without internal scrollbars.

With these steps, the Product Genie widget should load and function correctly on your page.
