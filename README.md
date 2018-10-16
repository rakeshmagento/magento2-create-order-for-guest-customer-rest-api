# magento2 create order for guest customer using rest api

### 1. Get the admin token :-
EndPoint : http://magento.host/index.php/rest/V1/integration/admin/token

Method : POST

Headers:

Content-Type : application/json
 
Body json:-

{"username":"API_USER","password":"API_PASSWORD"}

Response:-

16fsjy4242fssdsfsxipegpr9gpxf7p2424bx91sbbje0mg

This token will be used for the further steps while creating order through api.

### 2. Create empty cart for the guest customer :-

EndPoint : http://magento.host/index.php/rest/V1/guest-carts/

Method : POST

Headers:-

Content-Type  : application/json

Authorization : Bearer {TOKEN}

It will return a alphanumeric string, which is the cart id, this will be used to add/update/delete items form the cart.

### 3. Add/Update items in cart :-

Endpoint : http://magento.host/index.php/rest/V1/guest-carts/{cartId}/items
 
i.e. http://magento.host/index.php/rest/V1/guest-carts/4522ebc7d219rre415a8ad1eewr22f73b55/items
 
Method : POST

Headers:

Content-Type  : application/json

Authorization : Bearer {TOKEN}

Body json:

{
	"cartItem":
	{
	  "quote_id": "4522ebc7d219rre415a8ad1eewr22f73b55",
	  "sku": "Item SKU",
	  "qty": 1
	}
}

#### Note :- It will return the item_id which can be used later to update/delete item from the cart.

To update shopping item use the following request in body json on the same endpoint as above :-

{
  "cartItem":
  {
    "quote_id": "4522ebc7d219rre415a8ad1eewr22f73b55",
     "item_id": 1139,
     "sku": "Item SKU",
     "qty": 2
  }
}

### 4. Remove item from cart :-

EndPoint : http://magento.host/index.php/rest/V1/guest-carts/{cartId}/items/{itemId} 

As seen above the cart Id and ItemId, it should be looks like the following
 
http://magento.host/index.php/rest/V1/guest-carts/4522ebc7d219rre415a8ad1eewr22f73b55/items/1348
 
Method : DELETE

Headers:

Content-Type  : application/json

Authorization : Bearer {TOKEN}

Response : true

### 5. Get the cart details :-

EndPoint : http://magento.host/index.php/rest/V1/guest-carts/{CartId}

Method : GET

Headers:

Content-Type  : application/json

Authorization : Bearer {TOKEN}

### 6. Add shipping information to the cart :-

EndPoint : http://magento.host/index.php/rest/V1/guest-carts/{cartId}/shipping-information

Method : POST

Headers:

Content-Type  : application/json

Authorization : Bearer {TOKEN}

Body json :-

{
    "addressInformation": {
        "shippingAddress": {
            "country_id": "US",
            "street": [
                "Street Address"
            ],
            "company": "Company",
            "telephone": "2313131312",
            "postcode": "",
            "city": "California",
            "firstname": "john",
            "lastname": "harrison",
            "email": "guestuser@gmail.com",
            "sameAsBilling": 1
        },
        "billingAddress": {
            "country_id": "US",
            "street": [
                "Street Address"
            ],
            "company":"Company",
            "telephone": "2313131312",
            "postcode": "",
            "city": "California",
            "firstname": "john",
            "lastname": "harrison",
            "email": "guestuser@gmail.com"
        },
        "shipping_method_code": "flatrate",
        "shipping_carrier_code": "flatrate"
   }
}

#### Note :- Here flatrate is the shipping method code, you can change whatever shipping method you are using for your store.

It will return the payment methods available with entire cart information.

Response :-
{
    "payment_methods": [
        {
            "code": "checkmo",
            "title": "Check / Money Order"
        }
    ],
    "totals": {
        "grand_total": 40.5,
        "base_grand_total": 40.5,
        "subtotal": 40,
        "base_subtotal": 40,
        "discount_amount": 0,
        "base_discount_amount": 0,
        "subtotal_with_discount": 40,
        "base_subtotal_with_discount": 40,
        "shipping_amount": 0.5,
        "base_shipping_amount": 0.5,
        "shipping_discount_amount": 0,
        "base_shipping_discount_amount": 0,
        "tax_amount": 0,
        "base_tax_amount": 0,
        "weee_tax_applied_amount": null,
        "shipping_tax_amount": 0,
        "base_shipping_tax_amount": 0,
        "subtotal_incl_tax": 40,
        "shipping_incl_tax": 0.5,
        "base_shipping_incl_tax": 0.5,
        "base_currency_code": "USD",
        "quote_currency_code": "USD",
        "items_qty": 2,
        "items": [
            {
                "item_id": 14437,
                "price": 10,
                "base_price": 10,
                "qty": 2,
                "row_total": 40,
                "base_row_total": 40,
                "row_total_with_discount": 0,
                "tax_amount": 0,
                "base_tax_amount": 0,
                "tax_percent": 0,
                "discount_amount": 0,
                "base_discount_amount": 0,
                "discount_percent": 0,
                "price_incl_tax": 10,
                "base_price_incl_tax": 10,
                "row_total_incl_tax": 40,
                "base_row_total_incl_tax": 40,
                "options": "[]",
                "weee_tax_applied_amount": null,
                "weee_tax_applied": null,
                "name": "Baby Bag"
            }
        ],
        "total_segments": [
            {
                "code": "subtotal",
                "title": "Subtotal",
                "value": 40
            },
            {
                "code": "flatrate",
                "title": "Flat Rate",
                "value": 0.5
            },
            {
                "code": "tax",
                "title": "Tax",
                "value": 0,
                "extension_attributes": {
                    "tax_grandtotal_details": []
                }
            },
            {
                "code": "grand_total",
                "title": "Grand Total",
                "value": 40.5,
                "area": "footer"
            }
        ]
    }
}

### 7. Add payment information and place the order :-

EndPoint : http://magento.host/index.php/rest/V1/guest-carts/{cartId}/order

Method : PUT

Headers:

Content-Type  : application/json

Authorization : Bearer {TOKEN}

Body json:-

{
    "paymentMethod": {
       "method": "checkmo"
    }
}

It will return the order id some thing like 227.

Now you can check the order has been created in the admin.





