# Headless Shopping Cart

## Overview

Web components which convert any web page into a shopping cart.

This includes components to:-
 - Dynamically update price and qty for listed items. 
 - Add items to a cart and manage cart.
 - Authenticate and checkout.
 
Once customers are authenticated, price / qty shown and the types of payment / shipping methods available may change.
This functionality is usually specific to B2B e-commerce.

_For a website to use these components, it should include a single `.js` file and the appropriate components in the pages HTML._

## Web Components

Required web components

### Item Price

Displays the price of a item

```html
<s2s-item-price 
    sku="abc" 
    format="international" 
    currency="USD" />
```
e.g.

```html
<s2s-item-price >USD 1,234.56</s2s-item-price>
```

### Item Qty

Displays the available qty

```html
<s2s-item-qty 
    sku="abc" />
```
e.g.

```html
<s2s-item-qty >10</s2s-item-qty>
```

### Item Add

```html
<s2s-item-add 
    sku="abc"
    over-order="false"
    dynamic-price="true"
    dynamic-qty="true"
    description="Wonderful new shiny thing"
    title="Thing One"
    img="/some/image/link.jpg" />
```
Should create a clickable link if the item can be ordered

### Cart Checkout

```html
<s2s-cart-checkout />
```
Creates a link to checkout

### Cart Item Count

```html
<s2s-cart-item-count />
```
Summary of items in cart

### Cart Item Total

```html
<s2s-cart-item-total />
```
Summary of cart total

### Customer Account

```html
<s2s-customer-account />
```
Login link for customer to authenticate.
_TBC_

## JSON API Models 

Web components will need to communicate with an external API.
The below models would need to be implemented.

### User Authenticate

Fetch token for user. 

Example Endpoint:

`POST /users/authenticate`
```json
{
  "user": {
    "username": "string",
    "password": "string"
  }
}
```

Response Model Schema:

```json
{
  "user": {
    "id": "long",
    "email": "string",
    "name": "string",
    "surname": "string",
    "token": "string",
  }
}
```

### Items list

Verify a list of products and retrieve price / qty per product, if required. 

Example Endpoint:

`POST /items?token=xyz`

```
["sku 1", "sku 2", ...]
```

Response Model Schema:
```json
[
  {
    "sku": "string",
    "price": {
      "was": "number",
      "now": "number"
    },
    "qty": {
      "default": "number",
      "warehouse[x]": "number"
    }  
  }
]
```

### Order confirm

Verify that all items in the cart can be ordered.
Return shipping and payment methods.

Example Endpoint:

`POST /order/confirm`

```json
{
    TBC
}
```
 Model Schema:
 ```json
{
  TBC
}
 ```

### Order checkout

Post order for processing.

Example Endpoint:
```
POST /order/checkout
{
    TBC
}
```
 Model Schema:
 ```json
{
  TBC
}
 ```

### Order checkout

Post confirmation of payment.

Example Endpoint:
```
POST /payment
{
    TBC
}
```
 Model Schema:
 ```json
{
  TBC
}
 ```
