Web App Order Sync
==================

Integrating Shippo Web App Order Sync is the best option when you don't want to build your own shipping interface and strive to go to market quickly. Your users can use the Shippo web app to access all shipping features, and all order data will be synced automatically between your platform and Shippo.

You only need to push your users' orders from your system directly into the Shippo web app through our Orders endpoint---one simple POST API call per order. Your users can sign up for a Shippo account via OAuth and start shipping right away. You won't need to worry about building any shipping interfaces, billing flows, or customer onboarding.

Setup
-----

Each of your users needs to create their own Shippo account. To make the registration process as seamless as possible, we've built a lightweight express registration flow exclusively for platform partners. You can expose this registration process in an embedded iframe, popup, or new tab. The registration flow leverages OAuth and allows you to make API calls on behalf of your customers after they have signed up for Shippo. Your customer will automatically be redirected to your platform after they've signed up.

[**Get started with the OAuth registration process**](https://goshippo.com/docs/oauth/)

![](https://shippo-static.s3.amazonaws.com/img/various/oauth.gif)\
*Shippo OAuth registration page*

Building the Order Sync
-----------------------

### Pushing Orders into Shippo

To sync your customers' orders with Shippo, you can leverage our Orders API. At a minimum you want to implement the [API call to create orders within Shippo](https://goshippo.com/docs/orders/). This way, your customers can access all of their orders including the corresponding address, item, weight, price etc. information in Shippo without having to copy/paste any values manually.

Depending on how your platform works and what use cases you want to support, you can also update orders programmatically or retrieve packing slips from Shippo once labels have been created.

![](data:image/svg+xml;charset=utf-8,%3Csvg height='177' width='600' xmlns='http://www.w3.org/2000/svg' version='1.1'%3E%3C/svg%3E)

![](https://goshippo.com/_gatsby/image/10cf50d3f4e3579a94e6fa10a3015ff4/f611013b6afae59978734de6bd82a45c/weebly-orders.png?u=https%3A%2F%2Fwordpress-652598-2128589.cloudwaysapps.com%2Fwp-content%2Fuploads%2F2018%2F08%2Fweebly-orders.png&a=w%3D150%26h%3D44%26fm%3Dpng%26q%3D70)\
*Example of custom branding of Weebly orders with Weebly logo*

### Getting Label and Tracking Information from Shippo

Many platforms want to know when one of their customers has purchased a label on Shippo. As an example, you might want to store the corresponding carrier name and tracking number in your system, so that your users can track the package on your platform or send the tracking number via email to their buyers.

Shippo will automatically send you all information about a shipping label purchase if you subscribe to the `transaction_created` (creation only) or `transaction_updated` (any shipping label-related event) webhooks. We recommend that you subscribe to one of these webhooks and store any relevant data in your system once you receive the webhook.

You need to subscribe to the desired webhook(s) for each one of your customers individually. We offer a Webhooks API to allow you to programmatically create those webhooks once a customer connected their Shippo account with your platform via OAuth. The Webhooks API is currently in private beta -- please reach out to <partnerships@goshippo.com> to get access to the API.

We offer an account settings API endpoint that provides you control over your customer's Shippo account configuration, such as postage discount tier or pricing. Please reach out to <partnerships@goshippo.com> to discuss controllable settings.

Certified Partners
------------------

Select partners are given exclusive benefits on Shippo, including:

-   **Ability to brand orders in Shippo Web App:** show your own platform's logo on the Shippo Order Page in our Web App, instead of the Shippo logo.
-   **Platform listing:** list your platform as a Shippo integration directly on Shippo.

Please reach out to <partnerships@goshippo.com> to discuss certification.