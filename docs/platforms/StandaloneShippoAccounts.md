Standalone Shippo Accounts
==========================

Allowing your users to access Shippo through their own, standalone account is the best option if you want to offer shipping features within your platform, but don't want to handle the billing and customer support overhead.

Your users will sign up for a Shippo account through our OAuth flow, allowing them to easily sign up and connect their Shippo account. We bill and charge your users directly for any applicable fees, such as postage costs. We also support your customers directly with any shipping-related questions. Your customers can optionally use the full Shippo Web App, allowing you to focus on building only the most relevant features within your platforms.

Setup
-----

Each of your users needs to create their own Shippo account. To make the registration process as seamless as possible, we've built a lightweight express registration flow exclusively for platform partners. You can expose this registration process in an embedded iframe, popup, or new tab. The registration flow leverages OAuth and allows you to make API calls on behalf of your customers after they have signed up for Shippo. Your customer will automatically be redirected to your platform after they've signed up.

[**Get started with the OAuth registration process**](https://goshippo.com/docs/oauth/)

![](https://shippo-static.s3.amazonaws.com/img/various/oauth.gif)\
*Shippo OAuth registration page*

Building the Shipping UI
------------------------

### Basics

You need to decide which shipping features you want to support on your platform. Typically, our partners support these basic features:

-   [Shipping label purchase:](https://goshippo.com/docs/shipping-labels/) allow your customers to buy shipping labels from any carrier. Our Shipping Label API comes with hundreds of features, such as different label formats, address validation and much more. We also recommend supporting [international shipping label purchases](https://goshippo.com/docs/international/), which require customs declarations. If you don't need to display all available shipping services with their prices and transit times before label purchase, you can also [create shipping labels with a single API call](https://goshippo.com/docs/single-call/).
-   [Rating:](https://goshippo.com/docs/shipping-labels/) provide the ability to estimate shipping rates independent of purchasing a label. The most common use case is estimating shipping options/costs during checkout.
-   [Refunds:](https://goshippo.com/docs/refunds/) everyone makes mistakes and sometimes your customers might have made an error when purchasing a label. Allow them to void unused labels and get their money back.
-   [Address validation:](https://goshippo.com/docs/address-validation/) prevent failed deliveries and shipping errors by automatically validating any address worldwide with Shippo's Address Validation service.
-   [Tracking:](https://goshippo.com/docs/tracking/) keep your customers, and their buyers, informed about the status of their shipments. Engage buyers by proactively sending shipment and delivery notification, and stay on top of shipment delays or exceptions.

We also recommend that you make use of [webhooks](https://goshippo.com/docs/webhooks/) to update your customers about tracking updates in realtime and reduce your development time.

### Advanced Features

Depending on the type of platform and the sophistication of your customers' use cases, you might want to also implement some of the following functionality:

-   [Return labels](https://goshippo.com/docs/return-labels/) are oftentimes a core part of the shipping experience of e-commerce today. Allow your customers to create return labels that are only paid on use and can even be included in the outbound shipment for free.
-   [Batch label creation](https://goshippo.com/docs/batch/), which allows you to purchase thousands of shipping labels with a few API calls.
-   [Manifests](https://goshippo.com/docs/manifests/) are necessary for some carriers as end-of-day close-outs. Shipping providers like DHL eCommerce or Australia Post require you to create manifests for all of your shipments. This API endpoint also allows you to create USPS SCAN forms, if your customers prefer to use them.
-   [Shipping insurance](https://goshippo.com/docs/insurance/) allows your customers to avoid losses on lost or stolen packages by insuring them with our built-in insurance service or the carriers' insurance.
-   [Multi-piece shipments](https://goshippo.com/docs/multipiece/), which allow your customers to unlock carrier discounts for consolidated shipments to the same recipient.
-   Some customers also require extra shipment services like [dry ice](https://goshippo.com/docs/dryice/) or [alcohol](https://goshippo.com/docs/alcohol/).

We offer an account settings API endpoint that provides you control over your customer's Shippo account configuration, such as postage discount tier or pricing. Please reach out to <partnerships@goshippo.com> to discuss controllable settings.