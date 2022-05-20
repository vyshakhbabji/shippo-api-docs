White Label Integration
=======================

If you're looking for a seamless shipping integration, a white label integration is recommended. The integration is completely up to you since you own the entire experience. We have documented some of the most important aspects and considerations here to help you get started.

Setup
-----

To get started, you need your own Shippo account, which you will use as your platform's master account. It's a normal Shippo account, but you use it for all API calls on your platform. Shippo will bill this account for any charges that you incur.

[**Sign up for a Shippo Account now**](https://goshippo.com/register/)

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

Billing
-------

Because Shippo charges your Shippo account directly for all fees, you are responsible for billing your users directly. It's entirely up to you how and how much you're charging your users. Typically, our partners choose one of the following three billing models for their platform.

**1) Free or Flat-rate Shipping**\
If many of your shipments share the same characteristics, or if your platform includes any shipping fees in its core pricing model, you might decide not to charge your customer at all for shipping, or pass on a flat rate per label. As an example, marketplaces like Mercari offer a flat rate shipping model -- the seller can choose one weight tier and pays a flat rate price, regardless of the actual label price. This simplifies the customer experience and also reduces the development time since you don't have to worry about figuring out the exact shipping charges.

**2) Pass-through Shipping Costs**\
If you neither want to subsidize nor monetize shipping, you can transparently pass through all shipping costs directly to your customers. In this case, we recommend that you retrieve shipping rates (i.e. prizes) whenever you purchase a label -- either by allowing a user to rate shop (see tutorial for creating a label based on a list of shipping rates) or by retrieving the rate's `amount` field once a label has been purchased.

**3) Leverage Shipping as Revenue Driver**\
Some platforms decide to leverage shipping as a revenue driver. You can mark up shipping rates, for instance with a 10% markup of any shipping rate returned by Shippo, or a flat $0.50 markup per label. Alternatively, you can offer different shipping rates depending on which pricing plan your customers subscribe to, hence creating incentives to choose a more expensive pricing plan. Same as in option (2), we recommend that you retrieve shipping rates (i.e. prizes) whenever you purchase a label -- either by allowing a user to rate shop (see tutorial for creating a label based on a list of shipping rates) or by retrieving the rate's `amount` field once a label has been purchased.

**More questions? Reach out to <partnerships@goshippo.com>!**