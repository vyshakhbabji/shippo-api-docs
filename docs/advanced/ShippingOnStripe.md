Shipping on Stripe
==================

As of November 2019, Stripe deprecated its Orders API and is no longer accepting new customers. The below documentation remains to help legacy customers connect and troubleshoot. Questions? Please reach out to <support@goshippo.com>.

Through Shippo, you can now add automatic shipping rate calculations to your [Stripe](https://stripe.com/) orders. This tutorial will show you how to set up Shippo for Stripe and print labels using your pre-loaded order information.

Shippo is connected with [Stripe](https://stripe.com/) via their Orders API. If you're not familiar with the Stripe Order API, we recommend [visiting their documentation first.](https://stripe.com/docs/orders)

Setting up Shippo on Stripe
---------------------------

### Step 1: Create Your Shippo Account

To create your Shippo account and connect with Stripe:

1.  Sign up for a Shippo account. After signing up you will be taken to the Shippo dashboard.
2.  In the navigation bar under Settings>Integrations click [E-commerce Channels](https://app.goshippo.com/settings/connect) to open up the list of available integrations.
3.  Click the **Connect** button next to **Stripe**.
4.  Click "Show your callback URL"
5.  Copy the URL shown, you will need this URL to activate Shippo in Stripe.
6.  Click "Connect your Stripe account now"

By default, Shippo will use our discounted USPS and DHL Express to calculate shipping rates. To purchase shipping labels with these discounted rates, [connect with the Shippo shipping API using existing Stripe order information.](https://goshippo.com/docs/stripe#create-stripe-shipping-labels)

If you would like to use another carrier, you can configure additional options under [Carriers in the Shippo dashboard.](https://goshippo.com/user/carriers/) You will need to create an account with the carrier before you can set up the account in Shippo. [See the full list of supported carriers by Shippo.](https://goshippo.com/carriers/)

### Step 2: Activate Shippo in Stripe

To activate Shippo in your Stripe account:

1.  Log into your Stripe account and navigate to the [Orders Settings.](https://dashboard.stripe.com/account/relay/settings)
2.  Choose **Live mode** under **Settings**, then click **Change shipping** next to the **Shipping** setting.
3.  Choose **Provider** under **Type** and **Shippo** under Provider. [See our example.](https://shippo-static.s3.amazonaws.com/img/illustrations/stripe-relay-settings.png)
4.  Enter your full Shippo Stripe URL (it should start with **https://api.goshippo.com/stripe**) into the **Shippo URL** field in the form. (This is the URL you found on the Shippo API page.)
5.  Fill out the **From address** with the address you will be shipping orders from. Shippo will use this address as the source address when calculating shipping costs.
6.  Click **Update** to save your credentials, [as such.](https://shippo-static.s3.amazonaws.com/img/illustrations/stripe-relay-shipping.png)

If you want to test your Shippo integration before applying it to your production orders, select **Test Mode** in the Orders settings dropdown menu and enter your Shippo URL as described above.

Retrieving Shipping Rates on Stripe
-----------------------------------

After connecting Shippo on Stripe, Shippo will use the [addresses provided, weight of the SKUs, and the default (or custom) package dimensions](https://goshippo.com/docs/stripe#stripe-shipping-concepts) to calculate shipping costs across all the different carriers and service levels activated on your Shippo account.

You should automatically see multiple shipping methods show up in your order's `shipping_methods` array.

Stripe automatically picks the first rate by default, noted as `selected_shipping_method`. But you can allow your customer to pick any of the available shipping rates, and Stripe will automatically update their order total after the customer makes their selection.

Here's an example response from Stripe:

```
{
  "id": "or_18N3kWDAu10Yox5RuJQQpN69",
  "object": "order",
  "amount": 7559,
  ...
  "items": [
    {
      "object": "order_item",
      "amount": 6999,
      "currency": "usd",
      "description": "Happy Hippo Snacl",
      "parent": "happy_hippo_snack",
      "quantity": 1,
      "type": "sku"
    },
  ],
  ...
  "selected_shipping_method": "b3dfea87f39a4ea1a8cfb22f6ed0e057",
  "shipping_methods": [
      {
        "currency": "usd",
        "amount": 560,
        "delivery_estimate": {
          "date": "2016-06-23",
          "type": "exact"
        },
        "description": "USPS Priority Mail",
        "id": "b3dfea87f39a4ea1a8cfb22f6ed0e057"
      },
      {
        "currency": "usd",
        "amount": 2066,
        "delivery_estimate": {
          "date": "2016-06-24",
          "type": "exact"
        },
        "description": "USPS Priority Mail Express",
        "id": "f4577112bdd64f049ebc179d05d9ecec"
      },
  ],
  ...
}
```

Create Shipping Labels with Shippo
----------------------------------

You can use Shippo's API to purchase shipping labels with the discounted shipping rates calculated for your orders.

### Single Call Label Creation with the Selected Shipping Method

You will need to use the [single-call label creation](https://goshippo.com/docs/single-call) process to generate your shipping label.

The following is a step-by-step example of how to extract the `servicelevel_token` returned by the`select_shipping_method` from Stripe and use it as part of Shippo's label creation process.

Python

```
import shippo
import stripe

# replace  with your key
shippo.api_key = ""
stripe.api_key = ""

# get Stripe Order
order = stripe.Order.retrieve("")

# get Shippo Shipment
rate = shippo.Rate.retrieve(order.selected_shipping_method)
shipment = shippo.Shipment.retrieve(rate.shipment)
parcel = shippo.Parcel.retrieve(shipment.parcel)

# example address_from object dict
address_from = {
    "name": "Mr Hippo",
    "company": "Shippo",
    "street1": "215 Clayton St.",
    "city": "San Francisco",
    "state": "CA",
    "zip": "94117",
    "country": "US",
    "phone": "555 341 9393",
    "email": "mrhippo@goshippo.com",
}

# address_to object dict from Stripe Order
address_to = {
    "name": order.shipping.name,
    "street1": order.shipping.address.line1,
    "city": order.shipping.address.city,
    "state": order.shipping.address.state,
    "zip": order.shipping.address.postal_code,
    "country": order.shipping.address.country,
    "phone": order.shipping.phone,
    "email": "your@email.com",
}

# parcel object dict, getting weight from Shippo Shipment
parcel = {
    "length": "5",
    "width": "5",
    "height": "5",
    "distance_unit": "in",
    "weight": parcel.weight,
    "mass_unit": parcel.mass_unit,
}

# create Shipment dict
shipment = {
    "address_from": address_from,
    "address_to": address_to,
    "parcels": [parcel]
}

# create Label with one API call - we already know which service to use from the Rate
transaction = shippo.Transaction.create(
    shipment = shipment,
    carrier_account = rate.carrier_account,
    servicelevel_token = rate.servicelevel_token
)

print "Created Label with ID %s, tracking number %s and label URL %s" % (transaction.object_id, transaction.tracking_number, transaction.label_url)
```

Stripe Shipping Calculation Concepts
------------------------------------

When calculating shipping rates, Shippo requires the following information:

### Shipping Addresses

Shippo collects the following address information, programmatically sent by Stripe:

-   Your `address_from`
-   Customer's `address_to`

### Package Dimensions and Weight

Shipping rates depend on the size and weight of your package. We will be calculating the shipping rate for the box that you are sending off, not for individual SKUs.

Dimensions

Package dimensions have a significant impact on shipping rates. It is therefore important to provide dimensions that reflect the actual package you're going to use to ship the order.

To set your package sizes, visit the [Shippo dashboard](https://goshippo.com/user/settings/). There you'll be able to set the length, width, and height of your box and use this to calculate shipping rates.

If you do not do this, by default Shippo will use a box of 10" x 10" x 10" to calculate rates.

Weight

Shippo retrieves the package weight from the combined weight of all SKUs in the order. You can provide this information through the [Stripe Dashboard](https://dashboard.stripe.com/products) or the [Stripe API.](https://stripe.com/docs/api#product_object-package_dimensions)

To use the Stripe Dashboard, navigate to a product page and click the **Edit product** button. Make sure that the **Shippable** checkbox is checked and fill in the weight at the bottom of the page.

Without a weight, Shippo will not be able to return rates. However, if you'd like to set a default weight for your order, you may do so by visiting the [Shippo dashboard](https://goshippo.com/user/settings/) (where you had set your box dimensions).

In priority order, Shippo will calculate your shipping rate using the following weights:

1.  If you don't have a SKU weight on Stripe and no default weight on Shippo, you don't get rates.
2.  If you have SKU weight on Stripe and no default package on Shippo, we use the SKU weight.
3.  If you don't have SKU weight on Stripe but a default weight on Shippo, we use the default weight.
4.  If you have both, we use the SKU weight.