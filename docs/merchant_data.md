The following is the merchant_data that I used to make the request to create an order - containing the mandatory request parameters as described in the [Checkout API Reference](https://developers.klarna.com/api/#checkout-api ':target=_blank'):


```javascript
var merchant_data = {
    	"purchase_country": "GB",
    	"purchase_currency": "GBP",
    	"locale": "en-GB",
    	"order_amount": 50000,
    	"order_tax_amount": 0,
    	"order_lines": [{
    		"type": "physical",
    		"reference": "19-402-USA",
    		"name": "Red T-Shirt",
    		"quantity": 5,
    		"quantity_unit": "pcs",
    		"unit_price": 10000,
    		"tax_rate": 0,
    		"total_amount": 50000,
    		"total_discount_amount": 0,
    		"total_tax_amount": 0,
    		"merchant_data": "{'marketplace_seller_info':[{'product_category':'Women\'s Fashion','product_name':'Women Sweatshirt'}]}",
    		"product_url": "https://www.estore.com/products/f2a8d7e34",
    		"image_url": "https://www.exampleobjects.com/logo.png",
    		"product_identifiers": {
    			"category_path": "Electronics Store > Computers & Tablets > Desktops",
    			"global_trade_item_number": "735858293167",
    			"manufacturer_part_number": "BOXNUC5CPYH",
    			"brand": "Intel"
    		}
    	}],
    	"merchant_urls": {
    		"terms": "https://www.estore.com/terms.html",
    		"cancellation_terms": "https://www.estore.com/terms/cancellation.html",
    		"checkout": "https://www.estore.com/checkout.html",
    		"confirmation": "https://www.estore.com/confirmation.html",
    		"push": "https://www.estore.com/api/push",
    		"validation": "https://www.estore.com/api/validation",
    		"shipping_option_update": "https://www.estore.com/api/shipment",
    		"address_update": "https://www.estore.com/api/address",
    		"notification": "https://www.estore.com/api/pending",
    		"country_change": "https://www.estore.com/api/country"
    	}
    };
```