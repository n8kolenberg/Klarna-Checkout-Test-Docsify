## Getting the order_id

?> The ```order_id``` value received from Klarna is _**bd1f87b2-890b-4d4f-a35c-fe4d9702b168**_

To get the ```order_id``` value, I used Klarna's Checkout API endpoint and JavaScript to generate asynchronous requests.

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

> ```merchant_data``` in this case is an object containing the [mandatory merchant data](/merchant_data.md), checkout product details and a merchant object consisting of urls to which Klarna can redirect users after filling in the Checkout form. This is as described in the [Klarna Checkout API](https://developers.klarna.com/api/#checkout-api ':target=_blank') reference.

##### However, I stumbled upon 2 issues when doing so <br>

!> **CORS** - Cross Origin Resource Sharing Blocked by browser ad the resource is not on the same origin from which I made the request<br>

!>**401: Request Not Authorised** - It's stated in the Klarna API documentation that the username is not the same as the merchant ID, and that it consists of a Merchant ID combined with a random string. I therefore signed up for a test account and acquired test API Credentials.

?> _Test API Credentials:_<br>
Username: PK03011_25c46e54003e <br>
Password: 9DnLpqfMjAqOTkJ

### Trying with the new test API Credentials
I tried the request again and still stumbled on the Request not Authorized error. I researched what other methods there are to add authentication headers to the request and tried again but with no luck.

```javascript 
/* Testing with addition of headers object in Axios request containing Basic Authorization */
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



/* Testing with jQuery Ajax method and adding the headers before sending XHR request or as headers object */
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

![Alt Text 401 ](http://res.cloudinary.com/n8dawg/image/upload/v1531067091/401.png '401 Unauthorized')