# Pronatura Bydgoszcz

Support for schedules provided by [Pronatura Bydgoszcz](http://www.pronatura.bydgoszcz.pl/), serving Bydgoszcz, Poland.

There are other companies providing garbage collection services in Bydgoszcz (Remondis and Corimp) but this source does not support them.

To check which provider you have, visit this page at [Czysta Bydgoszcz](https://www.czystabydgoszcz.pl/odpady-komunalne/podzial-na-sektory/).

## Configuration via configuration.yaml

```yaml
waste_collection_schedule:
  sources:
    - name: pronatura_bydgoszcz_pl
      args:
        street_name: STREET_NAME
        street_number: STREET_NUMBER
```

**street_name**  
*(string) (required)*

**street_number**  
*(string|integer) (required)*

## Example

```yaml
waste_collection_schedule:
  sources:
    - name: pronatura_bydgoszcz_pl
      args:
        street_name: JÓZEFA SOWIŃSKIEGO
        street_number: 22A
```
