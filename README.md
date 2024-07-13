In this project, we will build a Spring Boot application to create a REST service for the 'Event Management System' application. The 'Event Management System' application is a bridge between events seeking sponsorship and potential sponsors looking for events to back. Users can navigate the platform to understand which sponsors are backing which events and the variety of events a particular sponsor might be associated with.

For the purposes of this application, we will define two primary entities: `Sponsor` and `Event`. The `Sponsor` entity has a Many-to-Many relationship with the `Event` entity.

<details>
<summary>**Implementation Files**</summary>

Use these files to complete the implementation:

- `SponsorController.java`
- `SponsorRepository.java`
- `SponsorJpaService.java`
- `SponsorJpaRepository.java`
- `Sponsor.java`
- `EventController.java`
- `EventRepository.java`
- `EventJpaService.java`
- `EventJpaRepository.java`
- `Event.java`

</details>

Create a database that contains three tables `sponsor`, `event`, and `event_sponsor` using the given database schema.

You can refer to this [session](https://learning.ccbp.in/course?c_id=e345dfa4-f5ce-406e-b19a-4ed720c54136&s_id=6a60610e-79c2-4e15-b675-45ddbd9bbe82&t_id=f880166e-2f51-4403-81a0-d2430694dae8), for creating a database.

_Create the SQL files and compose accurate queries to run the application. Inaccurate SQL files will result in test case failures._

<details>
<summary>**Database Schema**</summary>

#### Sponsor Table

| Columns  |                 Type                  |
| :------: | :-----------------------------------: |
|    id    | INTEGER (Primary Key, Auto Increment) |
|   name   |                 TEXT                  |
| industry |                 TEXT                  |

#### Event Table

| Columns |                 Type                  |
| :-----: | :-----------------------------------: |
|   id    | INTEGER (Primary Key, Auto Increment) |
|  name   |                 TEXT                  |
|  date   |                 TEXT                  |

#### Junction Table

|  Columns  |         Type          |
| :-------: | :-------------------: |
| sponsorId | INTEGER (Foreign Key) |
|  eventId  | INTEGER (Foreign Key) |

The columns `sponsorId` and `eventId` can be combinedly declared as Primary Keys.

You can use the given sample data to populate the tables.

<details>
<summary>**Sample Data**</summary>

#### Sponsor Data

|  id   |         name          |          industry          |
| :---: | :-------------------: | :------------------------: |
|   1   |       TechCorp        |         Technology         |
|   2   |     Glamour Inc.      |          Fashion           |
|   3   | SoundWave Productions |      Music Production      |
|   4   |       EcoPlanet       | Environmental Conservation |

#### Event Data

|  id   |         name          |    date    |
| :---: | :-------------------: | :--------: |
|   1   |        TechCon        | 2023-12-15 |
|   2   |     Fashion Fest      | 2023-11-05 |
|   3   |       MusicFest       | 2024-01-25 |
|   4   | EcoAwareness Conclave | 2023-11-10 |

#### Junction Table

| eventId | sponsorId |
| :-----: | :-------: |
|    1    |     1     |
|    1    |     2     |
|    2    |     2     |
|    3    |     3     |
|    3    |     4     |
|    4    |     4     |

</details>

</details>

<MultiLineNote>

Use only `sponsor`, `event`, and `event_sponsor` as the table names in your code.

</MultiLineNote>

### Completion Instructions

- `Sponsor.java`: The `Sponsor` class should contain the following attributes.

    |  Attribute  |     Type     |
    | :---------: | :----------: |
    |  sponsorId  |     int      |
    | sponsorName |    String    |
    |  industry   |    String    |
    |   events    | List\<Event> |

- `SponsorRepository.java`: Create an `interface` containing the required methods.
- `SponsorJpaService.java`: Update the service class with logic for managing sponsor data.
- `SponsorController.java`: Create the controller class to handle HTTP requests.
- `SponsorJpaRepository.java`: Create an interface that implements the `JpaRepository` interface.
  
- `Event.java`: The `Event` class should contain the following attributes.

    | Attribute |      Type      |
    | :-------: | :------------: |
    |  eventId  |      int       |
    | eventName |     String     |
    |   date    |     String     |
    | sponsors  | List\<Sponsor> |

- `EventRepository.java`: Create an `interface` containing the required methods.
- `EventJpaService.java`: Update the service class with logic for managing event data.
- `EventController.java`: Create the controller class to handle HTTP requests.
- `EventJpaRepository.java`: Create an interface that implements the `JpaRepository` interface.

Implement the following APIs.

<details>
<summary>**API 1: GET /events**</summary>

#### Path: `/events`

#### Method: `GET`

#### Description:

Returns a list of all events in the `event` table.

#### Response

```json
[
    {
        "eventId": 1,
        "eventName": "TechCon",
        "date": "2023-12-15",
        "sponsors": [
            {
                "sponsorId": 1,
                "sponsorName": "TechCorp",
                "industry": "Technology"
            },
            {
                "sponsorId": 2,
                "sponsorName": "Glamour Inc.",
                "industry": "Fashion"
            }
        ]
    },
    ...
]
```

</details>

<details>
<summary>**API 2: POST /events**</summary>

#### Path: `/events`

#### Method: `POST`

#### Description:

Creates a new event in the `event` table. Also, create an association between the event and sponsors in the `event_sponsor` table based on the `sponsorId`s provided in the `sponsors` field. The `eventId` is auto-incremented.

#### Request

```json
{
    "eventName": "Gaming Expo",
    "date": "2024-03-10",
    "sponsors": [
        {
            "sponsorId": 4
        }
    ]
}
```

#### Response

```json
{
    "eventId": 5,
    "eventName": "Gaming Expo",
    "date": "2024-03-10",
    "sponsors": [
        {
            "sponsorId": 4,
            "sponsorName": "EcoPlanet",
            "industry": "Environmental Conservation"
        }
    ]
}
```

</details>

<details>
<summary>**API 3: GET /events/{eventId}**</summary>

#### Path: `/events/{eventId}`

#### Method: `GET`

#### Description:

Returns an event based on the `eventId`. If the given `eventId` is not found in the `event` table, raise `ResponseStatusException` with `HttpStatus.NOT_FOUND`.

#### Success Response

```json
{
    "eventId": 1,
    "eventName": "TechCon",
    "date": "2023-12-15",
    "sponsors": [
        {
            "sponsorId": 1,
            "sponsorName": "TechCorp",
            "industry": "Technology"
        },
        {
            "sponsorId": 2,
            "sponsorName": "Glamour Inc.",
            "industry": "Fashion"
        }
    ]
}
```

</details>

<details>
<summary>**API 4: PUT /events/{eventId}**</summary>

#### Path: `/events/{eventId}`

#### Method: `PUT`

#### Description:

Updates the details of an event based on the `eventId` and returns the updated event details. Also update the associations between the event and sponsors, if the `sponsors` field is provided. If the given `eventId` is not found in the `event` table, raise `ResponseStatusException` with `HttpStatus.NOT_FOUND`.

#### Request

```json
{
    "eventName": "The Gaming Expo",
    "sponsors": []
}
```

#### Success Response

```json
{
    "eventId": 5,
    "eventName": "The Gaming Expo",
    "date": "2024-03-10",
    "sponsors": []
}
```

</details>

<details>
<summary>**API 5: DELETE /events/{eventId}**</summary>

#### Path: `/events/{eventId}`

#### Method: `DELETE`

#### Description:

Deletes an event from the `event` table and its associations from the `event_sponsor` table based on the `eventId` and returns the status code `204`(raise `ResponseStatusException` with `HttpStatus.NO_CONTENT`). If the given `eventId` is not found in the `event` table, raise `ResponseStatusException` with `HttpStatus.NOT_FOUND`.

</details>

<details>
<summary>**API 6: GET /events/{eventId}/sponsors**</summary>

#### Path: `/events/{eventId}/sponsors`

#### Method: `GET`

#### Description:

Returns all sponsors associated with the event based on the `eventId`. If the given `eventId` is not found in the `event` table, raise `ResponseStatusException` with `HttpStatus.NOT_FOUND`.

#### Success Response

```json
[
    {
        "sponsorId": 1,
        "sponsorName": "TechCorp",
        "industry": "Technology",
        "events": [
            {
                "eventId": 1,
                "eventName": "TechCon",
                "date": "2023-12-15"
            }
        ]
    },
    {
        "sponsorId": 2,
        "sponsorName": "Glamour Inc.",
        "industry": "Fashion",
        "events": [
            {
                "eventId": 1,
                "eventName": "TechCon",
                "date": "2023-12-15"
            },
            {
                "eventId": 2,
                "eventName": "Fashion Fest",
                "date": "2023-11-05"
            }
        ]
    }
]
```

</details>

<details>
<summary>**API 7: GET /events/sponsors**</summary>

#### Path: `/events/sponsors`

#### Method: `GET`

#### Description:

Returns a list of all sponsors in the `sponsor` table.

#### Response

```json
[
    {
        "sponsorId": 1,
        "sponsorName": "TechCorp",
        "industry": "Technology",
        "events": [
            {
                "eventId": 1,
                "eventName": "TechCon",
                "date": "2023-12-15"
            }
        ]
    },
    ...
]
```

</details>

<details>
<summary>**API 8: POST /events/sponsors**</summary>

#### Path: `/events/sponsors`

#### Method: `POST`

#### Description:

Creates a new sponsor in the `sponsor` table, if all the `eventId`s in the `events` field exists in the `event` table. Also, create an association between the sponsor and events in the `event_sponsor` table. The `sponsorId` is auto-incremented. If any given `eventId` is not found in the `event` table, raise `ResponseStatusException` with `HttpStatus.BAD_REQUEST`.

#### Request

```json
{
    "sponsorName": "GameOn Studios",
    "industry": "Video Gaming",
    "events": [
        {
            "eventId": 4
        }
    ]
}
```

#### Success Response

```json
{
    "sponsorId": 5,
    "sponsorName": "GameOn Studios",
    "industry": "Video Gaming",
    "events": [
        {
            "eventId": 4,
            "eventName": "EcoAwareness Conclave",
            "date": "2023-11-10"
        }
    ]
}
```

</details>

<details>
<summary>**API 9: GET /events/sponsors/{sponsorId}**</summary>

#### Path: `/events/sponsors/{sponsorId}`

#### Method: `GET`

#### Description:

Returns a sponsor based on the `sponsorId`. If the given `sponsorId` is not found in the `sponsor` table, raise `ResponseStatusException` with `HttpStatus.NOT_FOUND`.


#### Success Response

```json
{
    "sponsorId": 1,
    "sponsorName": "TechCorp",
    "industry": "Technology",
    "events": [
        {
            "eventId": 1,
            "eventName": "TechCon",
            "date": "2023-12-15"
        }
    ]
}
```

</details>

<details>
<summary>**API 10: PUT /events/sponsors/{sponsorId}**</summary>

#### Path: `/events/sponsors/{sponsorId}`

#### Method: `PUT`

#### Description:

Updates the details of a sponsor based on the `sponsorId` and returns the updated sponsor details. Also update the associations between the sponsor and events, if the `events` field is provided. If the given `sponsorId` is not found in the `sponsor` table, raise `ResponseStatusException` with `HttpStatus.NOT_FOUND`. If any given `eventId` is not found in the `event` table, raise `ResponseStatusException` with `HttpStatus.BAD_REQUEST`.

#### Request

```json
{
    "events": [
        {
            "eventId": 5
        }
    ]
}
```

#### Success Response

```json
{
    "sponsorId": 5,
    "sponsorName": "GameOn Studios",
    "industry": "Video Gaming",
    "events": [
        {
            "eventId": 5,
            "eventName": "The Gaming Expo",
            "date": "2024-03-10"
        }
    ]
}
```

</details>

<details>
<summary>**API 11: DELETE /events/sponsors/{sponsorId}**</summary>

#### Path:  `/events/sponsors/{sponsorId}`

#### Method: `DELETE`

#### Description:

Deletes a sponsor from the `sponsor` table and its associations from the `event_sponsor` table based on the `sponsorId` and returns the status code `204`(raise `ResponseStatusException` with `HttpStatus.NO_CONTENT`). If the given `sponsorId` is not found in the `sponsor` table, raise `ResponseStatusException` with `HttpStatus.NOT_FOUND`.

</details>

<details>
<summary>**API 12: GET /sponsors/{sponsorId}/events**</summary>

#### Path: `/sponsors/{sponsorId}/events`

#### Method: `GET`

#### Description:

Returns all events associated with the sponsor based on the `sponsorId`. If the given `sponsorId` is not found in the `sponsor` table, raise `ResponseStatusException` with `HttpStatus.NOT_FOUND`.

#### Success Response

```json
[
    {
        "eventId": 1,
        "eventName": "TechCon",
        "date": "2023-12-15",
        "sponsors": [
            {
                "sponsorId": 1,
                "sponsorName": "TechCorp",
                "industry": "Technology"
            },
            {
                "sponsorId": 2,
                "sponsorName": "Glamour Inc.",
                "industry": "Fashion"
            }
        ]
    }
]
```

</details>

**Do not modify the code in `EMSApplication.java`**