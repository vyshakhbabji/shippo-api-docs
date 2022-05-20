Returns
=======

Setup your reverse-logistics and returns easily through the API.

1.  [Scan-based return labels](https://goshippo.com/docs/return-labels#return-label): These labels are free to print. You won't get charged unless it gets used. It's a great option if you want to add return labels to all your outbound packages. All Shippo return labels are scan-based, available for USPS, FedEx, and UPS shipments only. At the moment, you can only generate return labels for the same carrier that your outbound shipment was sent with
2.  [Return address](https://goshippo.com/docs/return-labels#return-address): For failed deliveries or return shipments, you can specify a different return address than the initial outbound location

Generate a Return Label
-----------------------

To generate a pay-on-use return label, create a new Shipment object and set an `is_return`  field as `true` inside the extra attribute.

**Important:** do not swap the addresses yourself -- the Shippo API will take care of this for you. This means that the original `address_to` of the outbound transaction becomes the `address_from` for the return label.

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/shipments/\
    -H "Authorization: ShippoToken <API_Token>"\
    -d address_from="d799c2679e644279b59fe661ac8fa488"\
    -d address_to="42236bcf36214f62bcc6d7f12f02a849"\
    -d parcels=["7df2ecf8b4224763ab7c71fae7ec8274"]\
    -d shipment_date="2013-12-03T12:00:00.000Z"\
    -d extra='{ "is_return": true}'\
    -d async=false
```

After creating the Shipment, follow the [normal process of retrieving rates](https://goshippo.com/docs/shipping-labels/) to select the service level that you'd like to use for the return, and proceed to create your return label.

All scan-based return labels are valid for 365 days after they are generated.

### Returns for Other Carriers

If you need to generate a label for return shipments for carriers other than USPS, FedEx, and UPS, you can create a normal shipping label just with the addresses swapped.

However, normal shipping labels have a limited shipping window and the label may be rejected after that, so we do not recommend inserting them into your outbound package. Instead, you can provide a method for customers to contact your support services, and you can provide them with the link to the label.

Return Address
--------------

When creating an outbound Shipment for USPS, FedEx and UPS, you can specify a `address_return` that's different from your original shipping address (`address_from`). This can be useful if you want returned shipments to go back to a different place. For instance:

1.  Failed deliveries: the Shipment will be returned to a different facility than the original outbound destination.
2.  Scan-based labels: the Shippo API automatically swaps the `address_from` and `address_to` during the return label creation process. You can pass any address `object_id` or nested object in the corresponding fields -- they don't need to match the outbound addresses.

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/shipments/\
    -H "Authorization: ShippoToken <API_Token>"\
    -d address_from="d799c2679e644279b59fe661ac8fa488"\
    -d address_to="42236bcf36214f62bcc6d7f12f02a849"\
    -d address_return="1d3tb2c51chj77ci27b7dfne3fibp264"\
    -d parcels=["7df2ecf8b4224763ab7c71fae7ec8274"]\
    -d shipment_date="2013-12-03T12:00:00.000Z"\
    -d async=false
```