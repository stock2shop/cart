# Web Component Shopping Cart

## Overview

Web components to convert any web page into a shopping cart.

Web Components to:-
 - Dynamically update price and qty for items. 
 - Add items to a cart and manage cart.
 - Authenticate and checkout.
 
Authenticated customer may see price, qty, payment methods and shipping methods specific to them.
This functionality is usually associated with B2B e-commerce.

The components are designed to be used by different audiences:-
- Web developers with a basic understanding of HTML to quickly add e-commerce functionality to their site.
- Javascript software engineers who wish to integrate into different applications or have greater flexibility to design custom carts.    


Requirements for [SLC](https://herman.bearblog.dev/mvp-vs-slc/)

- Able to add the cart to a web page by including a single `.js` file and adding HTML.
- All components should be usable without requiring an external data source.
- Global data `store` can be set in various ways (external API, Web component props and data web components).
- Variation on products (products with options) must be supported.
- Dynamic loading of price and qty on page load
- Checkout process with the option for different types of confirmation
- Authentication and user login (no registration required) 

## Web Components

### Item Add

#### Basic example:

```html
<s2s-add sku="abc" />
```

Results in:

```html
<button class="s2s-add" onClick="javascript:s2s.cart.add('abc', 1)">Add to Cart</button>
``` 

#### Changing the layout:

```html
<s2s-add sku="abc" layout="link" />
```

Results in:

```html
<a class="s2s-add" href="javascript:s2s.cart.add('abc', 1)">Add to Cart</a>
```

```html
<s2s-add sku="abc" layout="img|/some/image/path.png" />
```

Results in:

```html
<a class="s2s-add" href="javascript:s2s.cart.add('abc', 1)"><img src="/some/image/path.png"/></a>
```
    
#### Variations

Some items have `options`.
For example, a shoe may come in different sizes, each size having its own sku with its own size `option`.
The shoe, consisting of multiple skus can then be displayed as one saleable item with a drop down to select the sku on size.

Here is an example setting variation with properties (note `skus` property and not `sku` is used)

```html
<s2s-add 
    skus="abc|edf|xyz"
    title="Brass Screw"
    option-length="30mm|40mm|50mm"
    option-thickness="6mm|6mm|8mm"
     />
```

The resulting HTML could look like below 
_TODO: confirm layouts for variants_

```html
<span class="s2s-add">
    <select>
        <option value="">Select Length</option>
        <option value="30mm">30mm</option>
        <option value="40mm">40mm</option>
        <option value="50mm">50mm</option>
    </select>
    <select>
        <option value="">Select Thickness</option>
        <option value="6mm">6mm</option>
        <option value="6mm">6mm</option>
    </select>
    <button onClick="javascript:s2s.cart.add('abc', 1)">Add to Cart</button>
</span>
```

#### Price and Available Qty

Price must be set for a sku for it to be saleable.
Qty (available qty) is not required, if omitted there is no limit to the amount which can be added to cart.  

```html
<s2s-add sku="abc" price="100" qty="10" />
```

Or in the case of variations:-

```html
<s2s-add 
    skus="abc|edf|xyz" 
    price="100|150|200" 
    qty="10|0|5" 
    option-size="s|m|l" />
```
#### Marketing 

Properties used for display

```html
<s2s-add 
    sku="abc"
    title="Thing One"
    description="Wonderful new shiny thing"
    img="/some/image/link.jpg"
    />
```

_TODO: define key value meta for display_

#### Input

_TODO: define possible form fields that can be used to capture additional info at checkout._
  
### CSV

A component to define data in a csv format, rather than using properties

All csv headers can be properties on `s2s-add`.
Regardless of which component you use, data is saved to a global state `store` and not in the component.
Potential ways to add data to global `store`:-
- via API
- via Web Component Props
- via csv Web Component
- via js script include

```html
<s2s-csv>
sku,qty,price
a,1,200
b,0,200.00
</s2s-csv>
```

### Item Price

Displays the price of a item

```html
<s2s-price sku="abc" format="international" currency="USD" />
```

Results in:

```html
<span class="s2s-price">1 234.00</span>
```

#### Currency and localisation

```html
<s2s-price 
    sku="abc" 
    format="international" 
    currency="USD" />
```

Results in:

```html
<span class="s2s-price">$1 234.00</span>
```

#### Multiple skus

if multiple sku's are given price can be displayed as a range.

Example:-
```html
<s2s-price 
    sku="abc|xyz" 
    format="international" 
    currency="AUD" />
```
Results in:


```html
<span class="s2s-price">from AUD 1 201.00 to AUD 1 500.00</span>
```

### Item Qty

Displays the available qty

```html
<s2s-qty sku="abc" />
```
Results in:

```html
<span class="qty">10</span>
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

Web components can communicate with an external API.

It should be possible to create a 3rd party API without heavy lifting.


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