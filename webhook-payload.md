---
layout: default
title: Payload
parent: Webhooks
nav_order: 2
---

# Events

Events are our way of letting you know when something interesting happens in your organisation. When an event occurs we create a new event payload and send it to your endpoint. For example when a job completes we create a `job.completed` event. The [object]("#object") is the resource associated with the event. For example in the `job.completed` event the object will contain a job.

Events are formatted as JSON. Below is a full description of the payload with the `data.object` omitted for brevity. A full description of the object can be found [here](#object)

```json
{
  "id": "63070be0372f1e7bc77962a6",
  "webhookId": "66b2e3ddb97f60b5270d5ce9",
  "organisationId": "657bf08c202997ba9d127922",
  "eventType": "job.completed",
  "dateTime": "2024-10-05T00:00:00Z",
  "data": {
    "object": {
        "objectType": "job",
    }
  },
  "version": 1
}
```

### Attributes
---
**id**  `string`\
Unique identifier for the EVENT. Note that this is NOT the identifier for the resource. 

---
**webhookId**  `string`\
Unique identifier for the webhook associated with this event. 

---
**organisationId**  `string`\
The organisation which this event belongs to.

---
**eventType**  `string`\
The name of the <a href="/#types-of-events">event type</a> (for example, `job.completed`). 

---
**dateTime**  `string`\
The UTC timestamp which this event occurred at. 

---
**version**  `string`\
The <a href="/#types-of-events">version</a> of this event. This will dictate the object found in the `data` field.

---
**data**  `object`\
The data contains both the object, and any other details associated with it. 

---
**data.object**  `object`\
The actual [object](#object) associated with this request. You can use the `objectType` field to identify what type of payload is there.

---
**data.object.objectType**  `string`\
The name of the type of object contained in the object field. This helps you identify what type of object is contained in the payload. 

# Object

The object is the resource associated with the event. For example in the `job.completed` event the object will contain a job. The `object.` You can use the `objectType` field to identify what type of payload is contained in it. Below is a list of all the object types. 

* [Job](#job)


## Job

The job both contains the summarised job object in the `object` and a file download to see the entire object. The summarised job does NOT contain the spray logs. The full job with all of the spray logs can be found. 

{: .warning}
Spray logs can be quite large (Megabyte scale) so the file download may take some time and take up quite a lot of space.


### Summarised Job

```json
{
  "object": {
    "objectType": "job",
    "id": "657bf08c202997ba9d127981",
    "status": "COMPLETE",
    "name": "Example Name",
    "description": "Example description",
    "startTime": "2024-10-04T00:00:00Z",
    "endTime": "2024-10-04T12:00:00Z",
    "createdAt": "2024-10-03T12:00:00Z",
    "lastUpdatedAt": "2024-10-05T00:00:00Z",
    "completedAt": "2024-10-05T00:00:00Z",
    "location": {
      "address": "123 Example Street, ExampleTown NSW 2000, Australia",
      "latitude": 0,
      "longitude": 0
    },
    "applicationType": "FOLIAR_HIGH_VOLUME",
    "weatherInformation": [
      {
        "temperature": {
          "value": 12.15,
          "units": "CELSIUS"
        },
        "windSpeed": {
          "value": 1.14,
          "units": "METRES_PER_SECOND"
        },
        "deltaT": {
          "value": 2.77,
          "units": "CELSIUS"
        },
        "windBearing": {
          "value": 68,
          "units": "DEGREE"
        },
        "humidity": {
          "value": 74,
          "units": "PERCENT"
        },
        "timestamp": "2024-05-27T22:48:00.000Z"
      }
    ],
    "volumeDispensed": {
      "value": 21.580,
      "units": "LITRE"
    },
    "nozzleWidth": {
      "value": 23,
      "units": "METER"
    },
    "areaSprayed": {
      "value": 1954.7895,
      "units": "SQUARE_METER"
    },
    "weedAssignments": [
      {
        "id": "650bcaf66d4b2ff364137a75",
        "name": "Example Weed 1"
      },
      {
        "id": "650bcc3793158510feb3cd75",
        "name": "Example Weed 2"
      },
      null
    ],
    "recipe": {
      "id": "650a575993158510feb3cd68",
      "name": "Example Recipe 1",
      "concentrations": [
        {
          "chemical": {
            "id": "650a56d193158510feb3cd65",
            "name": "Example Chemical 1",
            "type": "LIQUID"
          },
          "amount": {
            "value": 500,
            "units": "MILLILITRES_PER_HUNDRED_LITRES"
          }
        }
      ]
    },
    "deviceAssignments": [
      {
        "id": "66556436d275bbf621487915",
        "user": {
          "id": "82403745-756d-4d1d-8ad6-d2c25d5fcd26",
          "fullName": "John Doe"
        },
        "device": {
          "id": "868963047753117",
          "alias": "Example Device Alias"
        },
        "volumeDispensed": {
          "value": 5.84999942779541,
          "units": "LITRE"
        }
      }
    ]
  },
  "file": {
    "content-type": "application/json",
    "url": "https://app.rapidlogix.com/media/...",
    "expires": "2024-10-12T00:00:00Z"
  }
}
```

### Attributes

Note that all job attributes can be found [here](#attributes-2). Below simply describes the file attributes unique to the summarised job. 

---
**content-type**  `string`\
The <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types" target="_blank">MIME</a> content type of the file. In this cause this is always `application/json`

---
**url**  `string`\
The URL where the file download is located. This is a HTTPS URL. The download can be achieved by performing a simple HTTPS request. Note that this url is temporary and will be unusuable after the `expires` time. This Url is signed which ensures that it hasn't been tampered with and comes from us. 

---
**expires**  `string`\
The date and time which the `url` expires. After this time the url can't be used again. Currently the url is set to expire 7 days after the webhook event is pushed

{: .warning}
Once a URL expires, you won't be able to access the [Full Job](#full-job). Make sure you download and save it before this date.


### Full Job

The full job is **downloaded through the file located on the [summarised job](#summarised-job)**. This object does NOT appear on the event object payload.

```json
{
  "objectType": "job",
  "id": "657bf08c202997ba9d127981",
  "status": "COMPLETE",
  "name": "Example Name",
  "description": "Example description",
  "startTime": "2024-10-04T00:00:00Z",
  "endTime": "2024-10-04T12:00:00Z",
  "createdAt": "2024-10-03T12:00:00Z",
  "lastUpdatedAt": "2024-10-05T00:00:00Z",
  "completedAt": "2024-10-05T00:00:00Z",
  "location": {
    "address": "123 Example Street, ExampleTown NSW 2000, Australia",
    "latitude": 0,
    "longitude": 0
  },
  "applicationType": "FOLIAR_HIGH_VOLUME",
  "weatherInformation": [
    {
      "temperature": {
        "value": 12.15,
        "units": "CELSIUS"
      },
      "windSpeed": {
        "value": 1.14,
        "units": "METRES_PER_SECOND"
      },
      "deltaT": {
        "value": 2.77,
        "units": "CELSIUS"
      },
      "windBearing": {
        "value": 68,
        "units": "DEGREE"
      },
      "humidity": {
        "value": 74,
        "units": "PERCENT"
      },
      "timestamp": "2024-05-27T22:48:00.000Z"
    }
  ],
  "volumeDispensed": {
    "value": 21.58,
    "units": "LITRE"
  },
  "nozzleWidth": {
    "value": 23,
    "units": "METER"
  },
  "areaSprayed": {
    "value": 1954.7895,
    "units": "SQUARE_METER"
  },
  "weedAssignments": [
    {
      "id": "650bcaf66d4b2ff364137a75",
      "name": "Example Weed 1"
    },
    {
      "id": "650bcc3793158510feb3cd75",
      "name": "Example Weed 2"
    },
    null
  ],
  "recipe": {
    "id": "650a575993158510feb3cd68",
    "name": "Example Recipe 1",
    "concentrations": [
      {
        "chemical": {
          "id": "650a56d193158510feb3cd65",
          "name": "Example Chemical 1",
          "type": "LIQUID"
        },
        "amount": {
          "value": 500,
          "units": "MILLILITRES_PER_HUNDRED_LITRES"
        }
      }
    ]
  },
  "deviceAssignments": [
    {
      "id": "66556436d275bbf621487915",
      "user": {
        "id": "82403745-756d-4d1d-8ad6-d2c25d5fcd26",
        "fullName": "John Doe"
      },
      "device": {
        "id": "868963047753117",
        "alias": "Example Device Alias"
      },
      "volumeDispensed": {
        "value": 5.84999942779541,
        "units": "LITRE"
      },
      "sprayLogs": [
        {
          "t": "2022-12-22T00:33:47Z",
          "v": 
          "c": {
            "x": 151.8505859375,
            "y": -27.30297088623047,
            "z": {
              "value": 1397.6377952755904,
              "units": "METRE"
            }
          },
          "v": {
            "value":0.001,
            "units":"LITRE"
          },
          "w": 0
        }
      ]
    }
  ]
}
```

### Attributes
---
**objectType**  `string`\
As this is a job, this will always be `job`

---
**id**  `string`\
Unique identifier for the object.

---
**status**  `string`\
The current state of the job. Possible values: `INCOMPLETE`, `COMPLETE`, `FINALISED`.

---
**name**  `string`\
Name of the job.

---
**description**  `string`\
Description of the job.

---
**startTime**  `string`\
The UTC timestamp when the job started.

---
**endTime**  `string`\
The UTC timestamp when the job ended.

---
**createdAt**  `string`\
The UTC timestamp when the job was created.

---
**lastUpdatedAt**  `string`\
The UTC timestamp when the job was last updated.

---
**completedAt**  `string`\
The UTC timestamp when the job was completed.

---
**location**  `object`\
Details of the job's location.

---
**location.address**  `string`\
The address of the job location. This address is formatted as a standard australian postal address.

---
**location.latitude**  `number`\
The latitude of the job location.

---
**location.longitude**  `number`\
The longitude of the job location.

---
**applicationType**  `string`\
The method used to apply the chemical in the job. Possible values: `UNSPECIFIED`, `FOLIAR_LOW_VOLUME`, `FOLIAR_HIGH_VOLUME`, `SPOT_TREATMENT`, `BROADCAST`, `BASAL_BARK`, `SOIL_INJECTION`.

---
**weatherInformation**  `array`\
Array of weather information associated with the job.

---
**weatherInformation[].temperature**  `UnitMeasurement`\
The temperature value. Possible units: `CELSIUS`, `FAHRENHEIT`.

---
**weatherInformation[].windSpeed**  `UnitMeasurement`\
The speed of the wind. Possible units: `METRES_PER_SECOND`, `MILES_PER_HOUR`.

---
**weatherInformation[].deltaT**  `UnitMeasurement`\
The change in temperature throughout the job. Possible units: `CELSIUS`.

---
**weatherInformation[].windBearing**  `UnitMeasurement`\
The direction the wind was flowing. Possible units: `DEGREE`.

---
**weatherInformation[].humidity**  `UnitMeasurement`\
The humidity value. Possible units: `PERCENT`.

---
**weatherInformation[].timestamp**  `string`\
The UTC timestamp of the weather information.

---
**volumeDispensed**  `UnitMeasurement`\
The volume dispensed. Possible units: `LITRE`, `US_GALLON`.

---
**nozzleWidth**  `UnitMeasurement`\
The width of the nozzle used on the spray device. This may be null if no nozzle width is specified. Possible units: `Meter`, `Feet`.

---
**areaSprayed**  `UnitMeasurement`\
Total area sprayed. This may be null if no nozzle width is specified. Possible units: `HECTARE`, `ACRE`.

---
**weedAssignments**  `array`\
Signifies what weeds have been set to which buttons on devices for this job. Must be a contain 3 elements which can be null to signify no weed has been set for the relevant button.

---
**weedAssignments[].id**  `string`\
Unique identifier for the weed.

---
**weedAssignments[].name**  `string`\
Name of the weed.

---
**recipe**  `object`\
Recipe information associated with the job.

---
**recipe.id**  `string`\
Unique identifier for the recipe.

---
**recipe.name**  `string`\
Name of the recipe.

---
**recipe.concentrations**  `array`\
Array of concentration objects in the recipe.

---
**recipe.concentrations[].chemical**  `object`\
Chemical information in the recipe.

---
**recipe.concentrations[].chemical.id**  `string`\
Unique identifier for the chemical.

---
**recipe.concentrations[].chemical.name**  `string`\
Name of the chemical.

---
**recipe.concentrations[].chemical.type**  `string`\
Type of chemical. Possible values: `LIQUID`, `SOLID`.

---
**recipe.concentrations[].amount**  `UnitMeasurement`\
The amount of the chemical in the recipe. Possible units: `MILLILITRES_PER_HUNDRED_LITRES`, `FLUID_OUNCES_PER_US_GALLON`.

---
**deviceAssignments**  `array`\
A device assignment represents a user, using a device to complete a job. This is a list of all device assignments
associated with this job. 

---
**deviceAssignments[].id**  `string`\
Unique identifier for the device assignment.

---
**deviceAssignments[].user**  `object`\
The user who executed this device assignment. 

---
**deviceAssignments[].user.id**  `string`\
Unique identifier for the user.

---
**deviceAssignments[].user.fullName**  `string`\
Full name of the user. This is in the format `{first name} {last name}`

---
**deviceAssignments[].device**  `object`\
Device information associated with the device assignment.

---
**deviceAssignments[].device.id**  `string`\
Unique identifier for the device.

---
**deviceAssignments[].device.alias**  `string`\
Alias for the device.

---
**deviceAssignments[].volumeDispensed**  `UnitMeasurement`\
The total volume of solution dispensed for the device assignment. Note that this is the sum total across all the spray logs. Possible units: `LITRE`, `US_GALLON`.

---
**deviceAssignments[].sprayLogs**  `array`\
Array of spray log's associated with this device assignment.

---
**deviceAssignments[].sprayLogs[].t**  `string`\
The UTC timestamp of the spray log.

---
**deviceAssignments[].sprayLogs[].v**  `UnitMeasurement`\
The total volume of the solution dispensed between now and the previous log. Possible Units: LITRE, US_GALLONS

---
**deviceAssignments[].sprayLogs[].w** `number`\
The button(s) pressed when this spray log was captured. The value here maps to the `decimal` column in the following table. 

| Decimal | Button 1 | Button 2 | Button 3 |
|---------|----------|----------|----------|
|       0 |    false |    false |    false |
|       1 |     true |    false |    false |
|       2 |    false |     true |    false |
|       3 |     true |     true |    false |
|       4 |    false |    false |     true |
|       5 |     true |    false |     true |
|       6 |    false |     true |     true |
|       7 |     true |     true |     true |

For example, suppose this value were 4, that would mean that only button 3 were pressed. Similarly if the value were 6, then it would mean button 2 and button 3 were pressed.


---
**deviceAssignments[].sprayLogs[].c**  `object`\
The Gps coordinates of the device when this spray log was captured.

---
**deviceAssignments[].sprayLogs[].c.x**  `number`\
Latitude component of the coordinates in degrees.

---
**deviceAssignments[].sprayLogs[].c.y**  `number`\
Longitude component of the coordinates in degrees.

---
**deviceAssignments[].sprayLogs[].c.z**  `UnitMeasurement`\
The altitude of the coordinates. Possible units: `METRE`, `FEET`.

