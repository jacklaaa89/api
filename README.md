# API

This library is designed on top of the standard http library in go to add a lot of the boilerplate code which needs to be performed to consume an API.

I was originally developed for "https://github.com/jacklaaa89/pokeapi" but it felt right to move it into its own library where it can be used for other projects.

##### Generic API Client

My implementation of the generic API client handles the following:

* Logging
* Request timeouts.
* Retries on certain failures and applying a backoff strategy
* HTTP request caching (if the same request is seen in a short space of time, a cached version is returned)
* Wrapping and contextualising errors from the API
* Generic encoding / decoding
* Authentication
* Very trivial localization utilising the `Accept-Language` header

For a lot of the different components ive tried to provide multiple examples to demonstrate the flexibility
of each of them:

###### Encoders

Probably the most used component in the code, this allows us to control how requests are decoded
and how responses are encoded. They also allow us to flexibly set applicable headers based
on the encoded response. The current implementations in this example are: JSON and XML.

###### Loggers

I have provided a very simple logging interface and have provided implementations using
the `fmt` package in the standard library as well as a more comprehensive example using
the `zap` library (see: [here](https://github.com/uber-go/zap)).

###### Authentication

I have provided examples on how to authenticate using:

* Basic Authentication
* Bearer Token
* A custom header
* A custom query parameter variable

###### Backoff

I have provided two different retry backoff strategies which are:

* Constant - which applies the same backoff time between retries
* Exponential - which applies a exponential growth formula using the amount of retries to exponentially
  increase the timeout between retries.

Because I have made all of these features generic on a low-level client, any API client which utilises it
becomes very small and trivial. It means we can add more API providers and handle
more resources on existing API clients with very little effort, and any changes made to the generic client is
automatically reflected in any API client using it.
