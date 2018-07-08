# Hej!


> This is an overview of my thought process and understanding of Klarna's Checkout Test. </br> I've written it according to my best understanding of the checkout process. </br> All information in this documentation is based on my review of the Klarna developers documentation, research on StackOverflow, looking at the requests and responses on https://demo.klarna.com/ and troubleshooting the requests on my own test server.


#### Klarna's Checkout Process
For the intents of this documentation, we'll call our online user "Lagertha", and the site she's browsing greatstuff.com. Klarna's checkout process consists of the following steps:
* Lagertha added a couple of products to her basket on greatstuff.com and proceeds to checkout. A request gets sent to Klarna to create an order.
	* Within this order, the merchant sends along mandatory parameters s.a. merchant id, purchase product details, and merchant urls to store the order on the merchant's side, to allow for user redirection to terms & conditions, and confirmation page a.o.
* The response to this request contains a.o. an ```order_id```, and ```html_snippet``` (iframe) that renders the checkout widget
	* The merchant's page should include a div that will be populated with the ```html_snippet```
* Once the user completes the checkout form, the order gets updated with the user's information and the status of the order changes to complete
* The user is redirected to the confirmation page and is shown the Klarna Confirmation widget.

