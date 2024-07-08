# Bury Council

Support for schedules provided by [Bury Council](https://www.bury.gov.uk/), serving Bury, UK.

## Configuration via configuration.yaml

```yaml
waste_collection_schedule:
    sources:
    - name: bury_gov_uk
      args:
        id: PROPERTY_ID
        postcode: POSTCODE
        address: ADDRESS
```

### Configuration Variables

**id**<br>
*(string) (optional)*

**postcode**<br>
*(string) (optional)*

**address**<br>
*(string) (optional)*


## Example using UPRN

```yaml
waste_collection_schedule:
    sources:
    - name: bury_gov_uk
      args:
        id: "647186"
```

## Example using Address and Postcode

```yaml
waste_collection_schedule:
    sources:
    - name: bury_gov_uk
      args:
        address: "1 Oakwood Close"
        postcode: "BL8 1DD"
```

## How to find your `PROPERTY_ID`

Your PROPERTY_ID is the collection of numbers at the end of the url when viewing your collection schedule in Developer Tools on the Bury Council web site.

For example: https://www.bury.gov.uk/app-services/getPropertyById?id=647186

You have to navigate to https://www.bury.gov.uk/waste-and-recycling/bin-collection-days-and-alerts, open Dev Tools, Select Network and then input your Postcode and select your Address. The URL should appear as network traffic. 
