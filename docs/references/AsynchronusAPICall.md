Asynchronous API Response Handling
==================================

By default, the Shippo API returns rates and labels asynchronously.

We recommend to use synchronous responses for most implementations. In the current API version, you need to explicitly opt into sync responses by setting the `async` parameter in the POST body to `false`.

What are asynchronous responses?
--------------------------------

Asynchronous responses mean that Shippo won't be returning rates or label you have requested immediately. After your Rates or Label API POST call, you will get a successful response from the Shippo API, but without the actual rates or label. This allows your implementation to do other tasks while Shippo is retrieving your data.

Here's a sample schema for how asynchronous API responses work, in this case for Rate requests:

![](https://shippo-static.s3.amazonaws.com/img/various/async-calls.png)

When does it make sense to use asynchronous responses?
------------------------------------------------------

It takes time for Shippo to call upstream API(s), such as the USPS or FedEx API, to retrieve rates and/or label. By using asynchronous responses, your rates or label requests won't block the rest of your code, and you can proceed with other tasks in the meantime.

The best way to handle asynchronous API responses is to access the corresponding API resource (Rate or Transaction) one or multiple times after object creation. As soon as a carrier has returned a rate or label, it will be accessible in the corresponding Shippo API resource.