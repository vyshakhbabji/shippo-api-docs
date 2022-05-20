ePost Global
============

Special requirements for ePost Global domestic shipments

API users attempting to retrieve rates or purchase labels for ePost Global domestic shipments must specify an additional line_items field within the Parcel object. Below is a sample parcel payload with line_items specified:

```
{
	"length": 5,
	"width": 5,
	"height": 5,
	"distance_unit": "in",
	"weight": 3,
	"mass_unit": "lb",
	"line_items": [
  		{
  	"manufacture_country": "US",
    	"quantity": 1,
    	"weight_unit": "lb",
    	"weight": 1,
    	"title": "Jeans",
    	"amount": "30",
    	"sku": "HM-112",
    	"currency": "USD"
  	},
  	...
	]
  }
```

Create a new shipment as usual with the parcel to obtain a list of rate objects or use single call label creation to purchase labels. Note ePost Global international shipments do not require parcel.line_items.