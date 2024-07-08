# Wermelskirchen Abfallkalender

!!!! The IT service provider was hit by a disastrous cyber attack in October of 2023. Since then this API does not work anymore and might never in the future (at least in the same form). !!!!

Support for schedules provided by [Abfallkalender Wermelskirchen](https://www.wermelskirchen.de/rathaus/buergerservice/formulare-a-z/abfallkalender-online/) located in NRW, Germany.

## Limitations

The used api (ics) only provides future waste collection dates.  

## Configuration via configuration.yaml

```yaml
waste_collection_schedule:
  sources:
    - name: wermelskirchen_de
      args:
        street: Telegrafenstraße
        house_number: "10"
      customize:
        - type: Restabfall 2-woechentlich
          alias: Restabfall
          show: false
        - type: Restabfall 4-woechentlich
          alias: Restabfall
          show: true
        - type: Restabfall 6-woechentlich
          alias: Restabfall
          show: false
```

### Configuration Variables

**street**  
*(string) (required)*

**house_number**  
*(string) (required)*

## How to get the source arguments

Set your street and your house number. Should they not work, check on [Abfallkalender Wermelskirchen](https://www.wermelskirchen.de/rathaus/buergerservice/formulare-a-z/abfallkalender-online/) and use the closest entries.

Depending on your booked schedule for "Restabfall"/"Restmüll" you should set `show` in one of the types to true and the others to false.
