# Klarna APIs

## Order Management API

### Order

* Release remaining authorization:
	- POST /ordermanagement/v1/orders/{order_id}/release-remaining-authorization
	- inputs: int order_id
	- Description: 
		This API call is used to release the remaining authorized amount. This means you don't intend do any more captures and that the authorized amount can be returned to the customer.

* Extend authorization time:
	- POST /ordermanagement/v1/orders/{order_id}/extend-authorization-time
	- inputs: int order_id
	- Use this API call to extend the expiry time of an order. The possibility and length of order expiry extensions is set on a case by case basis in the contract with Klarna.

* Update customer addresses:
	- PATCH /ordermanagement/v1/orders/{order_id}/customer-details
	- inputs: shipping_address, billing_address
	- This API call is used to update the shipping and/or billing addresses of the customer.

* Cancel order:
	- POST POST /ordermanagement/v1/orders/{order_id}/cancel
	- inputs: order_id
	- This API call is used to cancel an order. Orders with captures can't be canceled and should instead be refunded.

* Update merchant references:
	- PATCH /ordermanagement/v1/orders/{order_id}/merchant-references
	- inputs: merchant_reference1, merchant_reference2
	- This API call is used to update one or both merchant references. If the key is set that reference will be overwritten. To clear a reference set the value to an empty string ("").

* Acknowledge order:
	- POST /ordermanagement/v1/orders/{order_id}/acknowledge
	- inputs: order_id
	- This API call is used by merchants to acknowledge the order. You will receive the order confirmation push until the order has been acknowledged.

* Get order:
	- GET /ordermanagement/v1/orders/{order_id}
	- inputs: order_id
	- This API call is used to read an order from Klarna. The order is only available using the order management API after the purchase has been completed.

* Set new order amount and order lines:
	- PATCH /ordermanagement/v1/orders/{order_id}/authorization
	- inputs: order_amount, description, order_lines
	- This API call is used to update the order amount and order lines. The new order_amount can not be negative, and can't be less than the current captured_amount.


### Refund

* Create refund:
	- POST /ordermanagement/v1/orders/{order_id}/refunds
	- inputs: refunded_amount, description, order_lines
	- This API call is used to refund an amount of a captured order. The refunded amount will be credited to the customer. The refunded amount must not be higher than captured_amount. The refunded amount can be accompanied by a descriptive text and order lines.
	  Note: The response code can vary (by region or by context). Never check for 201 or 204 specifically, use a more general check for 2xx responses.

* Get refund:
	- GET /ordermanagement/v1/orders/{order_id}/refunds/{refund_id}
	- inputs: none
	- This API call is used to read information on a refund.


### Capture

* Get one capture
	- GET /ordermanagement/v1/orders/{order_id}/captures/{capture_id}
	- inputs: order_id, capture_id
	- Use this API call to read information about a capture from Klarna

* Trigger resend of customer communication:
	- POST /ordermanagement/v1/orders/{order_id}/captures/{capture_id}/trigger-send-out
	- inputs: order_id, capture_id
	- Use this API call to trigger a new send out of customer payment communication, usually a new invoice, for a capture.

* Get all captures for one order:
	- GET /ordermanagement/v1/orders/{order_id}/captures
	- inputs
	- Use this API call to get all captures related to one order.

* Create capture:
	- POST /ordermanagement/v1/orders/{order_id}/captures
	- inputs: captured_amount, description, order_lines, shipping_info, shipping_delay
	- This API call is used to capture the order. Either the full order or parts of it. This call should be used when the order is fulfilled, i.e. when the goods are shipped to the customer.
	  Note that captured_amount must be equal to or less than the remaining_authorized_amount.

* Add shipping info to a capture:
	- POST /ordermanagement/v1/orders/{order_id}/captures/{capture_id}/shipping-info
	- inputs: shipping_info
	- Use this API call to add shipping info to a capture.


## Checkput API

* Create a new order:
	- POST /checkout/v3/orders
	- inputs: order model
	- Use this API call to create a new order. The response body will contain the full order object and the location header will contain the URL at which the newly created order can be found.

* Retrieve an order:
	- GET /checkout/v3/orders/{order_id}
	- inputs:
	- Use this API call to read an order from Klarna. 
	  Note that orders should only be read from the checkout API until the order is completed. Completed orders should be read using the order management API

* Update an order:
	- POST /checkout/v3/orders/{order_id}
	- inputs: order model
	- To update an order simply provide a JSON object with the properties you want to update. Properties not provided in the request will stay the same.
	  Note: an order can only be updated when the status is checkout_incomplete

### Callbacks

* Address update:
	- POST /{merchant_urls.address_update}
	- Will be called whenever the consumer changes billing or shipping address. The order will be updated according to the response received by Checkout.

* Country change:
	- POST /{merchant_urls.country_change}
	- Will be called whenever the consumer changes billing address country. The order will be updated according to the response received by Checkout.

* Shipping option update:
	- POST /{merchant_urls.shipping_option_update}
	- Will be called whenever the consumer selects a shipping option. The order will be updated according to the response received by Checkout.

* Order validation:
	- POST /{merchant_urls.validation}
	- Will be called before completing the purchase to validate the information provided by the consumer in Klarna's Checkout iframe.

## Payments API

### Sessions
### Orders

## Settlements API

### Payouts
### Transactions
### Reports

## Customer Tokens API

### Tokens

## Hosted Payment Page API

### Sessions

## Merchant Card Service API

### Virtual Credit Card Settlements