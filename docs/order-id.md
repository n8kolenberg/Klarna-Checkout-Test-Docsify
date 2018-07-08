## API Request to Create Order

?> The ```order_id``` value received from Klarna is _**2ee89230-ebf6-4437-a51f-cb23c2736a80**_

To get the ```order_id``` value, I used Klarna's Checkout API endpoint and AJAX to generate a ```POST``` request to create an order.

Particularly, I used the [Axios](https://github.com/axios/axios ':target=_blank') library to generate async requests.

```javascript

let url = 'https://api.playground.klarna.com/checkout/v3/orders';
let username = 'K500638';
let password = 'aicee*roh1pip2Pu';

axios.post(url, {
    data: JSON.stringify(merchant_data)
}, {
    auth: {
        username: username,
        password: password
    }

}).then(function (response) {
    console.log('Authenticated!');
    console.log(`Data: ${response}`);
}).catch(function (error) {
    console.log(`Error on Authentication: ${error}`);
});
```

> ```merchant_data``` in this case is an object containing the [mandatory merchant data](/merchant_data.md), checkout product details and a merchant object consisting of urls to which Klarna can redirect users after filling in the Checkout form. This is as described in the [Klarna Checkout API](https://developers.klarna.com/api/#checkout-api ':target=_blank') reference. <br>

___

## Blocking issues
##### However, I stumbled upon 2 issues when doing so <br>

!> **CORS** - Cross Origin Resource Sharing Blocked by browser ad the resource is not on the same origin from which I made the request<br>

?> **Solution**: Implemented [Chrome extension](https://chrome.google.com/webstore/detail/allow-control-allow-origi/nlfbmbojpeacfghkpbjhddihlkkiljbi ':target=_blank') allowing to request any site / resource with ajax from any source. It adds _'Allow-Control-Allow-Origin: *'_ header to the response.

!>**401: Request Not Authorised** - It's stated in the Klarna API documentation that the username is not the same as the merchant ID, and that it consists of a Merchant ID combined with a random string. I therefore signed up for a test account and acquired test API Credentials.

?> _Test API Credentials:_<br>
Username: PK03011_25c46e54003e <br>
Password: 9DnLpqfMjAqOTkJ <br>

___

## Additional Testing
### Trying with the new test API Credentials
I tried the request again and still stumbled on the Request not Authorized error. I researched what other methods there are to add authentication headers to the request and tried again but with no luck.

```javascript 
/*================================================== 
Testing with addition of headers object in Axios request 
containing Basic Authorization 
*/
let url = 'https://api.playground.klarna.com/checkout/v3/orders';
let username = 'PK03011_25c46e54003e';
let password = '9DnLpqfMjAqOTkJW';
let basicAuth = btoa(`${username} : ${password}`);
axios.post(url, {
    data: JSON.stringify(data)
}, {
    headers: {
        "Authorization": `Basic ${basicAuth}`
    }
}).then(function (response) {
    console.log('Authenticated!');
    console.log(`Data: ${response}`);
}).catch(function (error) {
    console.log(`Error on Authentication: ${error}`);
});



/*==================================================
 Testing with jQuery Ajax method and adding the headers 
 before sending XHR request or as headers object 
 */
$.ajax({
    headers: {
        "Authorization": `Basic ${basicAuth}`
    },
    url: 'https://api.playground.klarna.com/checkout/v3/orders',
    method: 'POST',
    contentType: "application/json",
    crossDomain: true,
    data: JSON.stringify(merchant_data),
    //   beforeSend: function (xhr){ 
    //         xhr.setRequestHeader('Authorization', make_base_auth(USERNAME, PASSWORD)); 
    //     },
}).done(function (response) {
    console.log("Data: ", response);
});
```
> Unfortunately, I still received the same error message:
![Alt Text 401](https://res.cloudinary.com/n8dawg/image/upload/v1531067091/401.png '401 Unauthorized')<br>

___ 

## Using Postman
I then used [Postman](https://www.getpostman.com/) to generate the request to the Klarna Checkout API as this tool doesn't have the same browser related restrictions.

?> I added the initially provided the username and password in the Authentication settings as Basic Auth.

![Alt Text](https://res.cloudinary.com/n8dawg/image/upload/v1531070848/postman_authentication.png 'Postman Auth Settings') 

?> Then, I added the required request parameters in JSON and sent the ```POST``` request to ```https://api.playground.klarna.com/checkout/v3/orders/``` <br>

![Alt Text](https://res.cloudinary.com/n8dawg/image/upload/v1531071446/postman_response.png ':target=_blank')

?>The response also contains the HTML Snippet needed to render the Klarna Checkout Widget :raised_hands: <br>

![Alt Text](https://res.cloudinary.com/n8dawg/image/upload/v1531071668/postman_response_html_snippet.png ':target=_blank')

#### Potential Victory GIF


___

## Merchant Data
The following is the merchant_data used in the ```POST``` request to create an order, containing the mandatory request parameters as described in the [Checkout API Reference](https://developers.klarna.com/api/#checkout-api ':target=_blank'):


```javascript
let merchant_data = {
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