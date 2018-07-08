# Hej!


> This is an overview of my thought process and understanding of Klarna's Checkout Test. </br> I've written it according to my best understanding of the checkout process. </br> All information in this documentation is based on my review of the Klarna developers documentation, research on StackOverflow, looking at the requests and responses on https://demo.klarna.com/ and troubleshooting the requests on my own test server.


#### Klarna's Checkout Process
For the intents of this documentation, we'll call our online user "Lagertha", and the site she's browsing greatstuff.com. Klarna's checkout process consists of the following steps:
* Lagertha added a couple of products to her basket on greatstuff.com and proceeds to checkout. A request gets sent to Klarna to create an order.
	* Within this order, the merchant (greatstuff.com) sends along optional and mandatory parameters s.a. merchant ID, purchase product details, and merchant urls to store the order on the merchant's side, to allow for Lagertha to be redirected to greatstuff.com's confirmation page a.o.
* The response by Klarna's Checkout API to this request contains a.o. an ```order_id``` value, and an ```html_snippet``` (iframe) that renders the checkout widget.
	* Greatstuff.com's checkout page should include a div that will be populated with the ```html_snippet``` which will include an iframe with KLarna's Checkout Widget.
* Once Lagertha completes the checkout form, the order gets updated with the her information and the status of the order changes to complete.
* Lagertha is redirected to the confirmation page, where another request is sent to Klarna's Checkout API requesting the full order details and is shown the Klarna Confirmation Widget due to the status of the order being complete.

### Overview of Checkout and Server to Server Communication
![logo](https://res.cloudinary.com/n8dawg/image/upload/v1531058456/s2s.png "User interaction with Merchant and Request and Response between Merchant and Klarna")


### Potential GIF  of success

