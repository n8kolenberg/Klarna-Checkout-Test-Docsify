# Hej! :wave:


> Thanks for taking the time to go through this documentation! This is an overview of my understanding of Klarna's Checkout Test and my thought process throughout the technical test. </br> All information in this documentation is based on my review of the Klarna Developers documentation and API Reference, research on StackOverflow, looking at the requests and responses on https://demo.klarna.com/ and troubleshooting the API requests on my own test server.


#### Klarna's Checkout Process
For this section, we'll call our online user _Lagertha_ :girl:, and the site she's browsing _estore.com_. 

#### Klarna's checkout process consists of the following (smooth) main steps:
* Lagertha adds a couple of products to her basket on estore.com and proceeds to checkout. <br>

?> The merchant (estore.com) sends an API request to Klarna to create an order. Within this request, the merchant (estore.com) sends along **optional and mandatory parameters** s.a. merchant ID, purchase product details, and merchant urls (used for Klarna to notify the merchant when to store the order on the merchant's systems, to allow for Lagertha to be redirected to estore.com's confirmation page, and to see the Ts&Cs a.o.)

* The response by Klarna's Checkout API to this request contains a.o. an ```order_id``` value, and an ```html_snippet``` value containing html for an iframe through which the checkout widget is rendered. The status of the order at this point is incomplete<br>

?>estore.com's checkout page should include a **div** that will be populated with the ```html_snippet``` which includes the ```iframe``` through which to render Klarna's Checkout Widget.

* Once Lagertha completes the Checkout form, the order gets updated with her information and the status of the order changes to complete. She is then redirected to the estore.com's confirmation url (_one of the merchant urls mentioned before_) with the order id appended to it. This allows for the merchant to generate another request to Klarna for the full order details. Lagertha is then shown the Klarna Confirmation Widget. <br>

?> Klarna sends a **push notification** to the merchant to have them store the order in their system. The merchant afterwards sends a request to Klarna's Order Management API to acknowledge that the order has been registered. 


### Overview of Checkout Process and Client to Server Communication
<p align="center">
  <img  width=500 height=500 src="https://res.cloudinary.com/n8dawg/image/upload/v1531058456/s2s.png">
</p>


### So SMOOOTH
<p align="center">
  <img  src="https://res.cloudinary.com/n8dawg/image/upload/v1531110521/fish.gif">
</p>


