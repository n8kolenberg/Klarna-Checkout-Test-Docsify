## API Request to Create Order

?> Firstly, the ```order_id``` value received from Klarna is _**2ee89230-ebf6-4437-a51f-cb23c2736a80**_

To get the ```order_id``` value, I used Klarna's Checkout API endpoint and AJAX to generate a ```POST``` request to create an order.

For generating AJAX requests, I used the [Axios](https://github.com/axios/axios ':target=_blank') library.

```javascript

let url = 'https://api.playground.klarna.com/checkout/v3/orders';
let username = 'XXXX';
let password = 'XXXX';

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

> ```merchant_data``` is an object containing the [mandatory merchant data](/order-id?id=merchant-data), checkout product details and a ```merchant_urls``` object consisting of urls to which Klarna can a.o. send the merchant a push notification signifying order completion and redirect users to the confirmation page after filling in the Checkout form. This is as defined in the [Klarna Checkout API](https://developers.klarna.com/api/#checkout-api ':target=_blank') reference. <br>

___

## Blocking issues
##### When performing the requests, I stumbled upon 2 issues: <br>

!> **CORS** - Cross Origin Resource Sharing blocked by the browser as the resource is not on the same origin from which I made the request<br>

?> **Solution**: Implemented [Chrome extension](https://chrome.google.com/webstore/detail/allow-control-allow-origi/nlfbmbojpeacfghkpbjhddihlkkiljbi ':target=_blank') allowing to request any site / resource with AJAX from any source. It adds _'Allow-Control-Allow-Origin: *'_ header to the response.

!>**401: Request Not Authorised** - It's stated in the [Klarna API reference](https://developers.klarna.com/api/#authentication ':target=_blank') that the username is not the same as the merchant ID, and that it consists of a merchant ID combined with a random string. I therefore signed up for a test account and acquired test API Credentials.

?> _Test API Credentials:_<br>
Username: XXXXX <br>
Password: XXXXX <br>

___

## Additional Testing
### Requests with New Test API Credentials
I tried the request again and still stumbled upon the Request Not Authorized error. I researched what other methods there are to add authentication headers to the request and tried again but with no luck.

```javascript 
/*================================================== 
Testing with addition of headers object in Axios request 
containing Basic Authorization 
*/
let url = 'https://api.playground.klarna.com/checkout/v3/orders';
let username = 'XXXX';
let password = 'XXXX';
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
 before sending XHR request OR as headers object 
 */
 
 function make_base_auth(user, password) {
    var tok = user + ':' + password;
    var hash = btoa(tok);
    return 'Basic ' + hash;
}
 
$.ajax({
    headers: {
        "Authorization": `Basic ${basicAuth}`
    },
    url: 'https://api.playground.klarna.com/checkout/v3/orders',
    method: 'POST',
    contentType: "application/json",
    crossDomain: true,
    data: JSON.stringify(merchant_data),
       beforeSend: function (xhr){ 
             xhr.setRequestHeader('Authorization', make_base_auth(username, password)); 
         },
}).done(function (response) {
    console.log("Data: ", response);
});
```
> Unfortunately, I still received the same error message:
![401](https://res.cloudinary.com/n8dawg/image/upload/v1531067091/401.png '401 Unauthorized')<br>

___ 

## Using Postman
I then used [Postman](https://www.getpostman.com/) to generate the request to the Klarna's Checkout API as this tool doesn't have the same browser related restrictions.

?> I added the initially provided username and password in the Authentication settings as Basic Auth.

![Postman Auth Settings](https://res.cloudinary.com/n8dawg/image/upload/v1531070848/postman_authentication.png 'Postman Auth Settings') 
___

?> Then, I added the required request parameters in JSON and sent the ```POST``` request to ```https://api.playground.klarna.com/checkout/v3/orders/```. The response is then successful and includes the ```order_id```! :tada: <br>

![JSON Body and Response](https://res.cloudinary.com/n8dawg/image/upload/v1531071446/postman_response.png 'JSON Body and Response')
___

?>The response also contains the HTML Snippet needed to render the Klarna Checkout form :raised_hands: <br>

![HTML_SNIPPET](https://res.cloudinary.com/n8dawg/image/upload/v1531071668/postman_response_html_snippet.png 'html_snippet in response')

---

<p align="center">
  <img src="https://res.cloudinary.com/n8dawg/image/upload/v1531076322/patrick_victory.gif">
</p>



___

## Merchant Data
The following is the ```merchant_data``` object I used in the ```POST``` request to create an order, containing the mandatory request parameters as described in the [Checkout API Reference](https://developers.klarna.com/api/#checkout-api ':target=_blank'):


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
