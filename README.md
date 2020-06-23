
### Overview

In this assignment, you will create a fake RoadMotion microservice to store vehicle data and perform some analysis. After that, you will describe how you would improve your system so the architecture can meet various requirements.

This assignment has the following set-up requirements:

* A computer running Linux, macOS, or Windows
* An internet connection
* A Python 3 installation

### Step 1

Create a Python script which listens to HTTP requests on port 3000. When the script receives a GET request at the endpoint `/greetUser`, the script should send back this JSON value:

```
{"success": true, "message": "Hello world!"}
```

We recommend using the Flask framework for your script. However, you may use a different framework if you are more comfortable using it.

### Step 2

Create a POST `/uploadSession` endpoint. The request body of this endpoint will contain a JSON dictionary with the following format:

```
{
    "id": number,
    "data": {
        "pos": [number, number]
        "speed": number
    }[]
}
```

This assignment includes the following example JSON session files:

* `session36255.json`
* `session35935.json`
* `session36261.json`

When the script receives an `/uploadSession` request, it should save the JSON data to a file in `./sessionFiles/session(ID).json`, where `(ID)` is the ID number in the JSON data. The `sessionFiles` directory should be in the same directory as your script. If the `sessionFiles` directory is missing, your script should create it.

If the JSON contains a valid integer ID, your script should provide the following response:

```
{"success": true}
```

If the ID is not a valid integer, your script should give this response:

```
{"success": false, "message": "ID is not a valid integer."}
```

We will assume for this whole assignment that the ID field is the only field which may be malformed in session JSON.

### Step 3

Create a GET `/sessionSpeedVariance` endpoint. The endpoint should accept a single query string parameter `id` which is the ID of a session. We want the endpoint to find the variance (as defined in statistics) of all speed values of the session.

We DON'T want you to create your own variance function. Instead, use the existing variance function in the NumPy library (https://numpy.org). Document the steps you took to install this dependency.

Upon success, the endpoint should give this response:

```
{"success": true, "speedVariance": number}
```

Below is the expected output for each example session file:

* `session36255.json` speed variance = 49.65
* `session35935.json` speed variance = 56.69
* `session36261.json` speed variance = 43.33

If the given session ID has no corresponding file, the endpoint should return this response:

```
{"success": false, "message": "Session does not exist."}
```

### Step 4

Create a GET `/sessionDistance` endpoint. This endpoint should also accept a single query string parameter `id`. We want the endpoint to find the distance covered by the whole session.

The position field `pos` first contains a latitude coordinate, then a longitude coordinate. The `/sessionDistance` endpoint should find the distance between consecutive `data` elements and return the sum of all the distances.

We DON'T want you to do any geometry. Instead, find an Python library which calculates the distance between two coordinate pairs. Document the name of the library and how you installed it.

Upon success, the endpoint should give this response:

```
{"success": true, "distance": number}
```

`distance` should be in meters. Below is the expected output for each example session file:

* `session36255.json` distance = 1415.1 meters
* `session35935.json` distance = 2800.4 meters
* `session36261.json` distance = 8206.7 meters

As in the previous step of this exercise, the endpoint should return this response if the session file is missing:

```
{"success": false, "message": "Session does not exist."}
```

### Step 5

The server we have implemented in steps 1 through 4 is rather primitive. For instance, the server can only run on a single machine because it saves session data to local files.

Suppose that we wanted to scale this application to support the following requirements:

* Must ingest session data from approximately 50,000 active vehicles
* Must handle approximately 500,000 calls to `/sessionSpeedVariance` and `/sessionDistance` per day
* Must be robust against data loss due to server failure
* Clients must authenticate before uploading or downloading data

Please describe how you would architect the system to satisfy these requirements. Discuss specific frameworks, database software, and hosting services your architecture would use. For each subsystem, explain the benefits and drawbacks of options you considered.

You will need to present your work in a **PowerPoint presentation** to the RoadMotion team. This presentation should describe your code from steps 1 through 4, and your proposed architecture from step 5. The audience will consist of multi-disciplined engineers (not just software developers), so make sure that the whole team can understand your presentation.


