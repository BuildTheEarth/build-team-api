# BTE Build Team API v1: Documentation

## Rules
Do not excessively use the API. The website server does not have endless resources and BTE does not have money to acquire state-of-the-art servers just for this.

## How to use the API
The Build Team API can only be used by build teams. 

### Getting your API key
Go to https://buildtheearth.net/buildteams/{id}/api-key (and )replace {id} with the ID of your build team). Generate an API key. You can always come back to this page to retrieve your key again, or invalidate it and generate a new key. 

### Making requests
All endpoints live on `https://buildtheearth.net/api/v1/....`. Requests must be made over HTTPS.
When making requests to the Build Team API, you must include the following HTTP headers:

    Host: buildtheearth.net
    Authorization: Bearer <api-key>
    Accept: application/json

## Endpoints
The following endpoints exists. If you think another endpoint is really necessary, send a message to Xesau#1681 on Discord.

### GET /api/v1/members
This endpoint returns a list of all members in the build team. It accepts no parameters.

#### Structure
The response will be an object with the property `members`, which is an array of objects with the following properties.
* `discordId`: integer, the Discord ID of the member
* `discordTag`: string, the full Discord tag of the member
* `role`: string, which can be `"leader"`, `"coleader`, `"reviewer"` or `"builder"`

#### Example response

    {
        "members": [
            {
                "discordId": 146312749146701824,
                "discordTag": "Xesau#1681",
                "role": "builder"
            },
            ...
        ]
    }

### GET /api/v1/locations
This endpoint returns a list of all builds of this team. It accepts no parameters.

#### Structure
The response will be an object with the property `locations`, which is an array of objects with the following properties.
* `id`: integer, the ID of the build
* `name`: string, the name of the build
* `regions`: integer, the amount of regions in the build
* `note`: string, the note which the build team (co-)leader has added to the build

#### Example response

    {
        "locations": [
            {
                "id: 39,
                "name": "Sometown",
                "regions": 53,
                "expansionPending": false,
                "note": "Someuser#4322 is working on it"
            },
            ...
        ]
    }

### GET /api/v1/locations/{id}/regions
This endpoint can be used to retrieve all region coordinates of a build.
The `{id}` parameter in the URL corresponds the `id` property of a build from `/api/v1/locations`.

#### Structure
The response will be an object with the property `regions`, which is an array of strings that represent region locations, in the format `x.y`.

#### Example response

    {
        "regions": [
            "42314.-2392",
            "42314.-2391",
            "42315.-2392"
        ]
    }


### GET /api/v1/applications/pending
This endpoint can be used to list all pending build team applications.

#### Structure
The response will be an object with the property `applications`, which is an array of objects with the following properties:
* `id`: integer, the ID of the application
* `user`: object, an object with the properties `discordTag` (string) and `discordId` (integer)
* `answers`: array, an array of objects with the properties
  * `id`: integer, the ID of the question,
  * `question`: string, the text of the question,
  * `answer`: string, the applicant's answer to the question

#### Example response

    {
        "applications": [
            {
                "id": 34251,
                "user": {
                    "discordTag": "Xesau#1681",
                    "discordId": 146312749146701824
                },
                "answers": [
                    {
                        "id": 3912,
                        "question": "Do you have much experience building homes?",
                        "answer": "Only a little"
                    },
                    ...
                ],
                "media": "https://imgur.com/a/....."
            }
        ]
    }

### GET /api/v1/ping
This endpoint can be used to see if a connection with the API has been made successfully. This endpoint accepts no parameters. The response will always an object with the property `ping`, whose value will be `"pong"`.

#### Example response

    {
        "ping": "pong"
    }
