# Web Component Shopping Cart

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

### Item Add

Enables items to be saleable.

```html
<s2s-add 
    sku="abc (required)"
    title="Thing One (required)"
    over-order="false (optional)"
    dynamic-price="true (optional)"
    dynamic-qty="true (optional)"
    price="123 (optional)"
    qty="10 (optional)"
    description="Wonderful new shiny thing (optional)"
    img="/some/image/link.jpg (optional)"
    option-size="6 (optional)"
    />
```

This HTML should create an "add to cart" button/link.
When clicked this will add the item to the cart and open the cart overlay (modal) interface.

Some items have options.
For example, a shoe may come in different sizes, each size having its own sku but it is 
displayed as one saleable item with a drop down to select the size.

```html
<s2s-add 
    skus="abc|xyz"
    title="Shoe"
    option-size="10|11"
     />

```

This goes further, you may have products with multiple options and different pricing and quantities for these options.
Example: 

```html
<s2s-add 
    skus="abc|edf|xyz"
    title="Brass Screw"
    price="0.34|0.40|0.56"
    qty="120|35|123"
    option-length="30mm|40mm|50mm"
    option-thickness="6mm|6mm|8mm"
     />

```
 
 The rules are:-
 - No two skus can have the same combination of options
 - Any web component property can be separated by `|` e.g. `over-order="false|false|true"` and will be assigned to the sku at the same index
 - Web component properties without a `|` will apply to all skus.
  

### Item Price

Displays the price of a item

```html
<s2s-price 
    sku="abc" 
    format="international" 
    currency="USD" />
```
if multiple sku's aree given price can be a range.
Example:-
```html
<s2s-price 
    sku="abc|xyz" 
    format="international" 
    currency="AUD" />
```
The result of which could be:


```html
<s2s-price >from AUD 1 201.00 to USD 1 500.00</s2s-price>
```


### Item Qty

Displays the available qty

```html
<s2s-qty 
    sku="abc" />
```
e.g.

```html
<s2s-qty >10</s2s-qty>
```


### Cart

```html
<s2s-cart />
```
Creates a link that when clicked opens a overlay (modal) cart which:-
- Allows for qty adjustment on items
- Allows for removing an item
- Shows pictures or any other required item meta data
- Has checkout link

### Cart Checkout

```html
<s2s-checkout />
```
Creates a link that when clicked opens a overlay (modal) checkout which:-
- Displays order summary
- Has billing / shipping address fields
- Has payment selection method (e.g. on account/cc)


### Cart Item Count

```html
<s2s-count />
```
Sum of items in cart

### Cart Item Total

```html
<s2s-total />
```
Summary of cart total

### Customer Account

```html
<s2s-customer />
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