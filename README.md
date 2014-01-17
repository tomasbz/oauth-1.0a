oauth-1.0a
==========

[![Dependencies](https://api.travis-ci.org/joeddo/oauth-1.0a.png)](https://travis-ci.org/joeddo/oauth-1.0a)
[![Dependency Status](https://gemnasium.com/joeddo/oauth-1.0a.png)](https://gemnasium.com/joeddo/oauth-1.0a)

OAuth 1.0a Request Authorizer for **Node** and **Browser**

Send OAuth request with your favorite HTTP client ([request](https://github.com/mikeal/request), [jQuery.ajax](http://api.jquery.com/jQuery.ajax/)...)

## Quick Start

Setup
```js
var oauth = new OAuth({
    consumer: {
        public: '<your consumer key>',
        secret: '<your consumer secret>'
    },
    signature_method: '<signature method>' //HMAC-SHA1 or PLAINTEXT ...
});
```

Get OAuth request data then you can use with your http client easily :)
```js
oauth.authorizer(request, token);
```

Or if you want to get as a header key-value data
```js
oauth.toHeader(oauth_data);
```


##Installation

###Node.js
    $ npm install oauth-1.0a
    
###Browser
Download oauth-1.0a.js [here](https://github.com/joeddo/oauth-1.0a/blob/master/lib/oauth-1.0a.js)

    <script src="http://crypto-js.googlecode.com/svn/tags/3.1.2/build/rollups/hmac-sha1.js"></script>
    <script src="http://crypto-js.googlecode.com/svn/tags/3.1.2/build/components/enc-base64-min.js"></script>
    <script src="oauth-1.0a.js"></script>

##Examples

###Work with [request](https://github.com/mikeal/request) (Node.js)

Depencies

```js
var request = require('request');
var OAuth   = require('oauth-1.0a');
```

Init
```js
var oauth = new OAuth({
    consumer: {
        public: 'xvz1evFS4wEEPTGEFPHBog',
        secret: 'kAcSOqF21Fu85e7zjz7ZN2U4ZRhfV3WpwPAoE3Z7kBw'
    },
    signature_method: 'HMAC-SHA1'
});
```

Your request data
```js
var request_data = {
	url: 'https://api.twitter.com/1/statuses/update.json?include_entities=true',
    method: 'POST',
    data: {
        status: 'Hello Ladies + Gentlemen, a signed OAuth request!'
    }
};
```

Your token
```js
var token = {
    public: '370773112-GmHxMAgYyLbNEtIKZeRNFsMKPR9EyMZeS9weJAEb',
    secret: 'LswwdoUaIvS8ltyTt5jkRh4J50vUPVVHtR2YPi5kE'
};
```

Call a request

```js
request({
	url: request_data.url,
	method: request_data.method,
	form: oauth.authorizer(request_data, token)
}, function(error, response, body) {
	//process your data here
});
```

Or if you want to send OAuth data as header

```js
request({
	url: request_data.url,
	method: request_data.method,
	form: request_data.data,
	headers: oauth.toHeader(oauth.authorizer(request_data, token))
}, function(error, response, body) {
	//process your data here
});
```

###Work with [jQuery.ajax](http://api.jquery.com/jQuery.ajax/) (Browser)

Init
```js
var oauth = new OAuth({
    consumer: {
        public: 'xvz1evFS4wEEPTGEFPHBog',
        secret: 'kAcSOqF21Fu85e7zjz7ZN2U4ZRhfV3WpwPAoE3Z7kBw'
    },
    signature_method: 'HMAC-SHA1'
});
```

Your request data
```js
var request_data = {
	url: 'https://api.twitter.com/1/statuses/update.json?include_entities=true',
    method: 'POST',
    data: {
        status: 'Hello Ladies + Gentlemen, a signed OAuth request!'
    }
};
```

Your token
```js
var token = {
    public: '370773112-GmHxMAgYyLbNEtIKZeRNFsMKPR9EyMZeS9weJAEb',
    secret: 'LswwdoUaIvS8ltyTt5jkRh4J50vUPVVHtR2YPi5kE'
};
```

Call a request

```js
$.ajax({
	url: request_data.url,
	type: request_data.method,
	data: oauth.authorizer(request_data, token)
}).done(function(data) {
	//process your data here
});
```

Or if you want to send OAuth data as header

```js
$.ajax({
	url: request_data.url,
	type: request_data.method,
	data: request_data.data,
	header: oauth.toHeader(oauth.authorizer(request_data, token))
}).done(function(data) {
	//process your data here
});
```
##Notes

**If you want a easier way to handle your OAuth request. Please visit [SimpleOAuth](https://github.com/joeddo/SimpleOAuth), it's a wrapper of this project, some features:**

* Request Token method
* Get Authorize link method
* Access Token method
* OAuth 2.0 support
* Simpler syntax:

Node.js:

```js
request(simple_oauth.do({
    method: 'GET',
    url: 'https://api.twitter.com/1.1/statuses/user_timeline.json'
}, function(error, response, body) {
	//process your data here
});
```

jQuery:

```js
$.ajax(simple_oauth.do({
    method: 'GET',
    url: 'https://api.twitter.com/1.1/statuses/user_timeline.json'
}.done(function(data) {
	//process your data here
});
```

##Todo
* RSA-SHA1 signature method

##Depencies
* Browser: [crypto-js](https://code.google.com/p/crypto-js/)
* Node: [crypto-js](https://github.com/evanvosberg/crypto-js)