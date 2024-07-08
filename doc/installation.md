<img src="/images/icon.png" alt="Waste Collection Schedule logo" title="Waste Collection Schedule" align="right" height="60" />

# Installation

## Automated Installation Using HACS

![hacs_badge](https://img.shields.io/badge/HACS-Default-orange)
![hacs installs](https://img.shields.io/endpoint.svg?url=https%3A%2F%2Flauwbier.nl%2Fhacs%2Fwaste_collection_schedule)

The `Waste Collection Schedule` component can be installed via [HACS](https://hacs.xyz/). This allows you to be notified of any updates or new releases of the component.

After installing HACS:

1. Visit the HACS `Integrations` panel in Home Assistant.
2. Click `Explore & Download Repositories`.
3. Search for `Waste Collection Schedule`.
4. Click on the `Waste Collection Schedule` entry.
5. Click on `Download` to copy the relevant files to your `config/custom_components/` directory.
6. [Configure your waste collection source(s)](#configuring-waste-collection-schedules).
7. [Configure your waste collection sensor(s)](#configuring-waste-collection-schedules).
8. Restart Home Assistant.

## Manual Installation

1. Navigate to the [waste_collection_schedule](https://github.com/mampfes/hacs_waste_collection_schedule/tree/master/custom_components) directory.
2. Copy the `waste_collection_schedule` folder (including all files and subdirectories) to your Home Assistant `config/custom_components/` directory.
3. [Configure your waste collection source(s)](#configuring-waste-collection-schedules).
4. [Configure your waste collection sensor(s)](#configuring-waste-collection-schedules).
5. Restart Home Assistant.

# Configuring Waste Collection Schedules

Waste Collection Schedule has two main configuration sections:

1. Configuring source(s)

   For each service provider, a source has to be added to the configuration. The source takes care of the arguments  required to get the correct information from the service provider's web page, e.g. district, city, street, house number, etc.

   If you have to fetch data from multiple service providers, you have to add multiple sources. You can also add the same service provider multiple times. This only makes sense if you use it with different arguments, e.g.  you are looking to display  waste collection schedules for multiple districts served by the same provider.

   If you prefer to combine the data into one calendar entity, you can use the [multiple](/doc/source/multiple.md) wrapper source. 

2. Configuring sensor(s)

   Sensors are used to visualize the retrieved information, e.g. waste type, next collection date, or number of days to next collection. The sensor state (which can be shown in a Lovelace/Mushroom cards) can be customized using templates. For example, you can display the collection type only, or the next collection date, or a combination of all available information.

   You can also add multiple sensors per source if you are going to display the information in separate entities. For example,  if you want each waste type to have its own entity, you can add one sensor per collection type.

## GUI Configuration

As of version 2.0.0 of the Waste Collection Schedule component, you can configure the component using the Home Assistant GUI. This is the recommended way for most configurations. There are a few limitations for advanced configurations, which can still be done using `configuration.yaml`.

### Adding Integrations

1. You can add sources and sensors like adding any other integration in Home Assistant. Just go to `Configuration` -> `Integrations` -> `Add Integration` and search for `Waste Collection Schedule`.
1. You will be asked to select your country (or Generic)
1. You will be asked to select your service provider
1. You will be asked to enter the required arguments for the service provider (you can find a detailed description of the arguments in the documentation of the source you selected (linked in the [README](/README.md#supported-service-providers))) and the name of the calendar.
1. You will be asked to select the waste types you to customize (you will be able to change the name, icon, and picture of the waste type and decide if you want to show or hide it). Detailed information can be found in the [Attributes for customize](#attributes-for-customize) section.
1. You will be asked to configure sensors. If you do not want to configure any sensors scroll down and select `Do not create sensor`. Otherwise configure the sensors as described in the [Configuring Sensor(s)](#configuring-sensors) section, if you want to configure multiple sensors, you must select `Add additional sensors` while configuring the previous sensor.
1. After completing the configuration you can click `CUSTOMIZE` to change some advanced settings described in the [Configuring Source(s)](#configuring-sources) section. You can also change the sensor or customizees here. Or click on the 3 dots and select reconfigure to change attributes of the source.

### GUI limitations

- You cannot define sensors in the YAML configuration that access the GUI sources (or the other way around)
- You cannot have sensors, that access multiple sources (source_index of YAML configuration)
- static source configuration is not ideal (but should work)
- `multiple` sources wrapper is currently not supported

## configuration.yaml

If you want to configure the Waste Collection Schedule using `configuration.yaml`, you can use the YAML snippet below

## Configuring Source(s)

```yaml
waste_collection_schedule:
  sources:
    - name: SOURCE
      args:
        arg1: ARG1
        arg2: ARG2
        arg3: ARG3
      customize:
        - type: TYPE
          alias: ALIAS
          show: SHOW
          icon: ICON
          picture: PICTURE
          use_dedicated_calendar: USE_DEDICATED_CALENDAR
          dedicated_calendar_title: DEDICATED_CALENDAR_TITLE
      day_offset: DAY_OFFSET
      calendar_title: CALENDAR_TITLE
  fetch_time: FETCH_TIME
  random_fetch_time_offset: RANDOM_FETCH_TIME_OFFSET
  day_switch_time: DAY_SWITCH_TIME
  separator: SEPARATOR
```

| Parameter | Type | Requirement | Description |
|-----|-----|-----|-----|
| sources | list | required | Contains information for the service provider being used. For details see [Attributes for sources](#attributes-for-sources) |
| fetch_time | time | optional | representation of the time of day in "HH:MM" that Home Assistant polls service provider for latest collection schedule. If no time is provided, the default of "01:00" is used |
| random_fetch_time_offset | int | optional | randomly offsets the `fetch_time` by up to _int_ minutes. Can be used to distribute Home Assistant fetch commands over a longer time frame to avoid peak loads at service providers |
| day_switch_time | time | optional | time of the day in "HH:MM" that Home Assistant dismisses the current entry and moves to the next entry. If no time if provided, the default of "10:00" is used. |
| separator | string | optional | Used to join entries if the multiple values for a single day are returned by the source. If no value is entered, the default of ", " is used |
| day_offset | int | optional | Offset in days to add to the collection date (can be negative). If no value is entered, the default of 0 is used |

## Attributes for _sources_

| Parameter | Type | Requirement | Description |
|-----|-----|-----|-----|
| name | string | required | name of the service provider source to use. Should be the same as the source filename, but without the `.py` extension. See the [README](/README.md#supported-service-providers) for supported service providers |
| args | various | required | source-specific arguments provided to service provider to unambiguously identify the collection schedule to return. Depending on the service provider, some arguments may be mandatory, and some may be optional. See individual sources for more details |
| customize | list | optional | Can be used to customise data retrieved from a source. For details see [Attributes for customize](#attributes-for-customize) |
| calendar_title | string | optional | A more readable, or user-friendly, name for the waste calendar. If nothing is provided, the name returned by the source will be used |

## Attributes for _customize_

| Parameter | Type | Requirement | Description |
|-----|-----|-----|-----|
| type | string | required | The identity of the waste type as returned from the source  |
| alias | string | optional | A more readable, or user-friendly, name for the type of waste being collected. Default is `None` |
| show | boolean | optional | Show (`True`) or hide (`False`) collections of this specific waste type. Default is `True` |
| icon | string | optional | Icon to use for this specific waste type. Icons from the Home Assistant mdi icon set can be used. Default is `None`. |
| picture | string | optional | string representation of the path to a picture used to represent this specific waste type. Default is `None` |
| use_dedicated_calendar | boolean | optional | Creates a calendar dedicated to this specific waste type. Default is `False` |
| dedicated_calendar_title | string | optional | A more readable, or user-friendly, name for this specific waste calendar object. If nothing is provided, the name returned by the source will be used |

## Configuring Sensor(s)

Add the following lines to your `configuration.yaml` file:

```yaml
sensor:
  - platform: waste_collection_schedule
    source_index: SOURCE_INDEX # (YAML only)
    name: NAME
    details_format: DETAILS_FORMAT
    count: COUNT
    leadtime: LEADTIME
    value_template: VALUE_TEMPLATE
    date_template: DATE_TEMPLATE
    add_days_to: ADD_DAYS_TO
    event_index: EVENT_INDEX
    types:
      - Waste Type 1
      - Waste Type 2
```

| Parameter | Type | Requirement | Description |
|--|--|--|--|
| platform |  | required | waste_collection_schedule |
| source_index *(YAML ONLY)* | int | optional | Used to assign a sensor to a specific source. Only needed if multiple sources are defined. The first source defined is source_index 0, the second source_index 1, etc. If no value is supplied, the default of 0 is used.<br><br>If you want to have a sensor which combines the data from multiple sources, just add a list of sources here. [Example](#combine-data-from-multiple-sources). This parameter is not available when using GUI configuration, as you're adding sensors directly to sources |
| name | string | required | The name Home Assistant used for this sensor |
| details_format | string | optional | Specifies the format used to display info in Home Assistant's _more info_ pop-up. Valid values are: `upcoming`, `appointment_types`, `generic` and `hidden`. If no value is supplied, the default of "upcoming" is used. See [options for details_format](#options-for-details_format-parameter) for more details |
| count | int | optional | Limits Home Assistant's _more info_ popup to displaying the next _int_ collections |
| leadtime | int | optional | Limits Home Assistant's _more info_ popup to only displaying collections happening within the next _leadtime_ days|
| value_template | string | optional | Uses Home Assistant templating to format the state information of an entity. See [template variables](#template-variables-for-value_template-and-date_template-parameters) for further details |
| date_template | string | optional | Uses Home Assistant templating to format the dates appearing within the _more info_ popup information of an entity. See [template variables](#template-variables-for-value_template-and-date_template-parameters) for further details |
| add_days_to | boolean | optional | Adds a `daysTo` attribute to the source entity state containing the number of days to  the next collection |
| event_index | int | optional | Used to assign a sensor to a specific pickup date index. The next pickup date has event_index 0. Useful if you want to have dedicated sensors for next collection, second collection, third collection, ... |
| types | list of strings | optional | Used to filter waste types. The sensor will only display collections matching these waste types. You need to use the alias if you used `alias` in the customize section of the sources configuration. |

## Options for _details_format_ parameter

Possible choices:

| upcoming | appointment_types | generic | hidden |
|--|--|--|--|
| shows a list of upcoming collections |shows a list of waste types and their next collection date | provides all attributes as generic Python data types. | hide attributes of upcoming collections |
| ![Upcoming](/images/more-info-upcoming.png) | ![Waste Types](/images/more-info-appointment-types.png) | ![Generic](/images/more-info-generic.png) | |

## Template variables for _value_template_ and _date_template_ parameters

The following variables can be used within `value_template` and `date_template`:

| Variable | Description | Type |Comments |
|--|--|--|--|
| `value.date` | Collection date | [datetime.date](https://docs.python.org/3/library/datetime.html#datetime.date) | Use [strftime](https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior) to format the output |
| `value.daysTo` | Days to collection | int  | 0 = today, 1 = tomorrow, etc |
| `value.types`  | Waste types | list of strings | Use `join` filter to join types |

Examples:

```yaml
    value_template: '{{value.types|join(", ")}}'
    value_template: 'in {{value.daysTo}} days'
    value_template: 'on {{value.date.strftime("%a")}}, {{value.date.strftime("%d.%m.%Y")}}'
```

## Combine Data from multiple Sources

### Combine Sensor Data (YAML only)

To combine data from multiple sources into one sensor, just add the source indexes like that:

```yaml
    source_index: [0, 1]
#or
    source_index:
      - 0
      - 1
```

### Combine Source Data (Not yet supported in GUI)

If you prefer to combine the data into one calendar entity, you can use the [multiple](/doc/source/multiple.md) wrapper source.

## HomeAssistant Service to manually trigger update

If you want to trigger a manual update of the sources, you can call the service:

`waste_collection_schedule.fetch_data`

Normally the configuration option 'fetch_time' is used to do this periodically.

## Further Help

For a full example, see [custom_components/waste_collection_schedule/waste_collection_schedule/source/example.py](/custom_components/waste_collection_schedule/waste_collection_schedule/source/example.py).

For other examples on how to configure source(s) and sensor(s), see the [FAQ](/doc/faq.md).
