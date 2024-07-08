# Stadtreinigung Leipzig

Stadtreinigung Leipzig is supported by the generic [ICS](/doc/source/ics.md) source. For all available configuration options, please refer to the source description.


## How to get the configuration arguments

- Goto <https://stadtreinigung-leipzig.de/wir-kommen-zu-ihnen/abfallkalender>, select your location and click on "Termine anzeigen".  
- Download the iCal file by clicking on 'Exportieren' -> `Ganztätig` -> `Herunterladen`.
- Copy the download link of the ical file (firefox: downloads menu -> right click -> copy download-link).
- Replace the `url` in the example configuration with this link.

## Examples

### Sandgrubenweg 27

```yaml
waste_collection_schedule:
  sources:
    - name: ics
      args:
        regex: (.*), .*
        url: https://stadtreinigung-leipzig.de/wir-kommen-zu-ihnen/abfallkalender/ical.ics?position_nos=38296&name=Sandgrubenweg&mode=download
```
