# Appointment API

## Appointment

### Overview

The appointment resource contains information about a planned meeting between a patient and medical provider. Examples include new patient encounters, scheduled surgeries and and follow-up visits.

### Fields

| Name | Description | Type |
| ---- | ---------- | ----------- |
| identifier | The unique value assigned to each appointment which discerns it from all others | [Identifier](https://www.hl7.org/fhir/datatypes.html#Identifier) |
| status | Indicates whether the appointment is cancelled, booked (confirmed), pending, arrived, fulfilled or no show | [AppointmentStatus](https://www.hl7.org/fhir/valueset-appointmentstatus.html) |
| description | The appointment summary that includes the type and a list of purposes and resources | [string](https://www.hl7.org/fhir/datatypes.html#string) |
| start | The date and time the appointment begins | [instant](https://www.hl7.org/fhir/datatypes.html#instant) |
| end | The date and time the appointment ends | [instant](https://www.hl7.org/fhir/datatypes.html#instant) |
| comment | The appointment notes | [string](https://www.hl7.org/fhir/datatypes.html#string) |
| participant | The collection of appointment participants which includes patient, provider and location. Each element in the collection references an embedded resource found in the “contained” collection field | [BackboneElement](https://www.hl7.org/fhir/backboneelement.html) |

### Sample
<pre class="center-column">
{
  "resourceType": "Bundle",
  "entry": [
    {
      "resource": {
        "resourceType": "Appointment",
        "id": "12312",
        "contained": [
          {
            "resourceType": "Location",
            "id": "5",
            "identifier": [
              {
                "use": "official",
                "value": "5"
              }
            ],
            "name": "South Dermatology"
          },
          {
            "resourceType": "Practitioner",
            "id": "2",
            "identifier": [
              {
                "use": "official",
                "value": "2"
              }
            ],
            "name": [
              {
                "text": "Davison, Darren",
                "family": "Davison",
                "given": [
                  "Darren",
                  ""
                ]
              }
            ]
          }
        ],
        "identifier": [
          {
            "use": "official",
            "value": "12312"
          }
        ],
        "status": "booked",
        "description": "Consult - Insurance Est. - Skin Lesion/Tumor for Darren Davis, MD",
        "start": "2017-03-12T09:10:00-04:00",
        "end": "2017-03-12T09:40:00-04:00",
        "comment": "This is a sample comment",
        "participant": [
          {
            "actor": {
              "reference": "Patient/4AAE9E3C-B1E4-46EA-93C2-CF3B36747D1A",
              "display": "Brimley, Henry"
            }
          },
          {
            "actor": {
              "reference": "#5",
              "display": "South Dermatology"
            }
          },
          {
            "actor": {
              "reference": "#2",
              "display": "Davison, Darren"
            }
          }
        ]
      }
    }
  ]
}
</pre>
&nbsp;
### *Search*
Searches for all appointments matching the given search criteria. See https://www.hl7.org/fhir/search.html for instructions on formatting search criteria.

#### HTTP Request 
`GET /Appointment?{parameters}`

#### Parameters
| Name | Located in | Description | Required |
| ---- | ---------- | ----------- | -------- |
| identifier | query | The unique identifier for a single appointment | No |
| patient | query | The unique identifier acquired from the patient search or a patient chart number | No |
| date | query | The appointment start date. When searching for appointments within a date range, the following prefixes may be used: lt, gt, eq, le, ge | No |


#### Example: Get all appointments for a patient by chart number

<pre class="center-column">
GET https://select.nextech-api.com/api/Appointment?patient=12345
</pre>

#### Example: Get all appointments between and including 1/1/2017 through 5/31/2017

<pre class="center-column">
GET https://select.nextech-api.com/api/Appointment?date=ge2017-01-01&date=lt2017-06-01
</pre>
&nbsp;
### *Confirm Appointment*
Confirm a scheduled appointment.

#### HTTP Request 
`PUT /Appointment/{identifier}` 


#### Parameters
| Name | Located in | Description | Required |
| ---- | ---------- | ----------- | -------- |
| identifier | path | The unique identifier of the appointment to be updated | Yes |
| commit | body | The object representing the changes to be made | Yes |

The commit parameter should be in the form of an Appointment status field. See https://www.hl7.org/fhir/http.html#update for details on formatting PUT requests in RESTful API’s.

#### Example: Mark appointment 5453 as booked

<pre class="center-column">
PUT https://select.nextech-api.com/api/Appointment/5453
</pre>
<pre class="center-column">
{ "status": "booked" }
</pre>
