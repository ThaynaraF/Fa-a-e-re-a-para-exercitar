## Updating SKU Specifications with Catalog API 

In our Catalog, [specifications](https://help.vtex.com/tracks/catalog-101--5AF0XfnjfWeopIFBgs3LIQ/2NQoBv8m4Yz3oQaLgDRagP?_ga=2.198453679.1025854910.1648393546-1926092690.1643042302) are created in the category level. Products and SKUs inherit their specifications from the category they are placed in and all its parent categories. This may lead to some confusion when using our API to create and update SKUs.

In this article, we share a few tips to help you update an [SKU Specification](https://help.vtex.com/tracks/catalog-101--5AF0XfnjfWeopIFBgs3LIQ/2NQoBv8m4Yz3oQaLgDRagP?_ga=2.197208495.1025854910.1648393546-1926092690.1643042302#sku-specifications).

### SKU and its Specification
First, you need to select an SKU and its Specification to update. If you don’t know the Specifications of the SKU, use the [Get SKU Specifications endpoint](https://developers.vtex.com/vtex-rest-api/reference/catalog-api-get-sku-specification). It will retrieve all Specifications indexed on the SKU.

In the example below, we show what the response would look like if your SKU had two specifications to fill in:

- Color ("FieldId": 271)
- Size ("FieldId": 40)

If needed, you would be able to get more information about each specification using the Get Specification Field endpoint.


```
Example: Get SKU Specifications

[
    {
        "Id": 472,
        "SkuId": 17784,
        "FieldId": 271,
        "FieldValueId": null,
        "Text": ""
    },
    {
        "Id": 528,
        "SkuId": 17784,
        "FieldId": 40,
        "FieldValueId": 147,
        "Text": "L"
    }
```

>:warning:
Notice that the response body from this endpoint is an array but you can’t update all at once. You can update only one SKU Specification at a time.

### Knowing the FieldValueId
After selecting the one Specification to update, use the [Get Specifications](https://developers.vtex.com/vtex-rest-api/reference/catalog-api-get-specification-field-value-fieldid) Values By Field Id endpoint. It allows you to know all the possible values from this Specification as you can only update the FieldValueId from an SKU Specification.

In the example below, we show what the response would look like if your specification had three possible values:

- S (```"FieldValueId": 144```)
- M (```"FieldValueId": 145```)
- L (```"FieldValueId": 146```)
Example: Get Specifications Values By Field Id
```
[
    {
        "FieldValueId": 144,
        "Value": "S",
        "IsActive": true,
        "Position": 1
    },
    {
        "FieldValueId": 145,
        "Value": "M",
        "IsActive": true,
        "Position": 2
    },
    {
        "FieldValueId": 147,
        "Value": "L",
        "IsActive": true,
        "Position": 3
    }
]
```
### Updating the SKU Specification FieldValueId
Finally, use the Update [SKU Specification](https://developers.vtex.com/vtex-rest-api/reference/put_api-catalog-pvt-stockkeepingunit-skuid-specification) endpoint to change the FieldValueId from the SKU Specification to the desired new value.


>:warning:
SKU Specifications can only have the FieldType as 5(Combo) or 6(Radio). You cannot change
the Text field without updating the FieldValueId. After this update, the Text field will be automatically modified
