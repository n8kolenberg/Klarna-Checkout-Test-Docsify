# Getting the order_id


``` JAVASCRIPT
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
```

