## Limitations
Due to using Postman to generate the requests, the ```html_snippet``` returns within the tool and can therefore not be dynamically generated into the designated div on my test site.

---

## Manual Render Trials
>I took the ```html_snippet```, cleaned it up and added it to my test server, and the Klarna Checkout Widget does then indeed render :ok_hand: <br>

![Checkout Widget Renders](https://res.cloudinary.com/n8dawg/image/upload/v1531073336/Checkout.png 'Checkout Widget Renders')
___

!> Unfortunately, due to the authentication issues previously explained and as I'm using test urls for the ```merchant_urls``` object, I'm not able to successfully submit the Checkout form data and therefore cannot render the Klarna Confirmation dynamically :disappointed:

![Same Auth Error](https://res.cloudinary.com/n8dawg/image/upload/v1531073942/error_authentication.png 'Same Authentication Error')

___

> After filling in the Checkout form, Klarna would send a push notification to the merchant via the ```merchant_urls.push``` url in the [merchant_urls](/order-id?id=merchant-data) object in the ```POST``` request. This push notification is for the merchant to create the order in their system and can then acknowledge to Klarna that the order has been registered by sending a ```POST``` request to ```https://api.playground.klarna.com/ordermanagement/v1/orders/{order_id}/acknowledge```.
<br>

> I tried using the ```order_id``` and performing a ```GET``` request to ```https://api.playground.klarna.com/ordermanagement/v1/orders/2ee89230-ebf6-4437-a51f-cb23c2736a80``` to get the html snippet returned for the Confirmation widget, but I unfortunately get an error stating that no such order exists. <br>
I get the same error message when trying to mimick a ```POST``` request as a merchant to acknowledge the order as described above.

![No such order exists](https://res.cloudinary.com/n8dawg/image/upload/v1531075828/no_such_order.png 'No such order exists?')

___


## Conclusion
?> Unfortunately, due to the issues mentioned, I was not able to dynamically generate the Klarna Checkout Form or the Confirmation Form. I believe that implementing one of the Klarna SDKs would allow for these issues to be resolved. However, since the Developer Reference stated that it was possible to send requests to the API via JavaScript, I chose this direction for the test. <br> Regardless, it was an interesting opportunity and experience for me to learn more about Klarna's Checkout process and the technology behind it. I would really like to continue this learning process with Klarna.

___


## Cleaned up html_snippet
The following is the ```html_snippet``` I used after cleaning it up. It was received as a response from the Klarna Checkout API _create a new order_ request.
```html
<div id="klarna-checkout-container" style="overflow-x: hidden;">
    <div id="klarna-unsupported-page">
        <style type="text/css">
            @-webkit-keyframes klarnaFadeIn {
                from {
                    opacity: 0
                }
                to {
                    opacity: 1
                }
            }

            @-moz-keyframes klarnaFadeIn {
                from {
                    opacity: 0
                }
                to {
                    opacity: 1
                }
            }

            @keyframes klarnaFadeIn {
                from {
                    opacity: 0
                }
                to {
                    opacity: 1
                }
            }

            #klarna-unsupported-page {
                opacity: 0;
                opacity: 19;
                -webkit-animation: klarnaFadeIn ease-in 1;
                -moz-animation: klarnaFadeIn ease-in 1;
                animation: klarnaFadeIn ease-in 1;
                -webkit-animation-fill-mode: forwards;
                -moz-animation-fill-mode: forwards;
                animation-fill-mode: forwards;
                -webkit-animation-duration: .1s;
                -moz-animation-duration: .1s;
                animation-duration: .1s;
                -webkit-animation-delay: 5s;
                -moz-animation-delay: 5s;
                animation-delay: 5s;
                text-align: center;
                padding-top: 64px
            }

            #klarna-unsupported-page .heading {
                font-family: Source Sans Pro, Helvetica, Arial, sans-serif;
                line-height: 48px;
                font-weight: 200;
                color: #303030;
                font-size: 42px;
                margin: 24px 0
            }

            #klarna-unsupported-page .subheading {
                font-family: Source Sans Pro, Helvetica, Arial, sans-serif;
                line-height: 28px;
                font-weight: 400;
                color: rgba(0, 0, 0, .7);
                font-size: 19px;
                max-width: 560px;
                margin: 10px auto
            }

            #klarna-unsupported-page .subheading a {
                text-decoration: none;
                background-color: transparent;
                border: 0;
                color: rgba(0, 0, 0, .7);
                font-weight: 600
            }
        </style>
        <h1 class="heading">Oops.</h1>
        <p class="subheading">It seems you might be using an older web browser which is not safe nor compatible with modern web sites. For a smoother
            checkout experience, please install a
            <a href="http://outdatedbrowser.com/en" target="_blank">newer browser</a>.</p>
        <p class="subheading">Sorry for any inconvenience. For any questions, please contact customer service at
            <a href="https://www.klarna.com">Klarna.com</a>.</p>
    </div>
    <script type="text/javascript">
        /* <![CDATA[ */
        (function (w, k, i, d, n, c, l) {
            w[k] = w[k] || function () {
                (w[k].q = w[k].q || []).push(arguments)
            };
            l = w[k].config = {
                container: w.document.getElementById(i),
                ORDER_URL: 'https://checkout-eu.playground.klarna.com/yaco/orders/2ee89230-ebf6-4437-a51f-cb23c2736a80',
                AUTH_HEADER: 'KlarnaCheckout iqiige2dx5j9kv2p4s9f',
                LAYOUT: 'desktop',
                LOCALE: 'en-GB',
                ORDER_STATUS: 'checkout_incomplete',
                MERCHANT_TAC_URI: 'https://www.estore.com/terms.html',
                MERCHANT_TAC_TITLE: 'K500638',
                MERCHANT_NAME: 'K500638',
                MERCHANT_COLOR: '',
                GUI_OPTIONS: [],
                ALLOW_SEPARATE_SHIPPING_ADDRESS: false,
                PURCHASE_COUNTRY: 'gbr',
                PURCHASE_CURRENCY: 'GBP',
                DISALLOW_SKIP_NIN: false,
                ANALYTICS: '',
                TESTDRIVE: true,
                CHECKOUT_DOMAIN: 'https://checkout-eu.playground.klarna.com',
                ASSETS_DOMAIN: 'https://checkout.klarnacdn.net',
                BOOTSTRAP_SRC: 'https://a.klarnacdn.net/kcoc/180704-045fc0a/checkout.bootstrap.js',
                PREFILLED: false,
                DEVICE_RECOGNITION_URL: 'https://checkout-eu.playground.klarna.com/yaco/orders/2ee89230-ebf6-4437-a51f-cb23c2736a80/device_recognition',
                CLIENT_EVENT_HOST: 'https://evt.playground.klarna.com'
            };
            n = d.createElement('script');
            c = d.getElementById(i);
            n.async = !0;
            n.src = l.BOOTSTRAP_SRC;
            c.appendChild(n);
            try {
                ((w.Image && (new w.Image)) || (d.createElement && d.createElement('img')) || {}).src = l.CLIENT_EVENT_HOST +
                    '/v1/checkout/snippet/load' +
                    '?sid=' + l.ORDER_URL.split('/').slice(-1) +
                    '&order_status=' + w.encodeURIComponent(l.ORDER_STATUS) +
                    '&timestamp=' + (new Date).getTime();
            } catch (e) {}
        })(this, '_klarnaCheckout', 'klarna-checkout-container', document);
        /* ]]> */
    </script>
    <noscript>
        Please
        <a href="http://enable-javascript.com">enable JavaScript</a>.
    </noscript>
</div>
```




