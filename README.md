
### Overview

In this assignment, you will be creating a fake RoadMotion microservice to store vehicle data and perform some analysis. This assignment has the following set-up requirements:

* A computer running Linux, macOS, or Windows
* An internet connection
* A Node.js installation (At least 8.0.0)

### Step 1

Create a Node.js script which listens to HTTP requests on port 3000. When the script receives a GET request at the endpoint `/greetUser`, the script should send back this JSON value:

```
{"success": true, "message": "Hello world!"}
```

We recommend using the Express framework for your script. However, you may use a different framework if you are more comfortable using it.

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

We DON'T want you to create your own variance function. Instead, find and use an existing npm library.

Upon success, the endpoint should give this response:

```
{"success": true, "speedVariance": number}
```

Below is the expected output for each example session file:

* `session36255.json` speed variance = 49.65
* `session35935.json` speed variance = 56.69
* `session36261.json` speed variance = 43.33

Upon failure, the endpoint should give this response:

```
{"success": false, "message": string}
```

For example, the script should report failure if the given session ID has no corresponding file. Try to include as many failure conditions as possible within reason.

### Step 4

Create a GET `/sessionDistance` endpoint. This endpoint should also accept a single query string parameter `id`. We want the endpoint to find the distance covered by the whole session.

The position field `pos` first contains a latitude coordinate, then a longitude coordinate. The `/sessionDistance` endpoint should find the distance between consecutive `data` elements and return the sum of all the distances.

We DON'T want you to do any geometry. Instead, find an npm library which calculates the distance between two coordinate pairs.

Upon success, the endpoint should give this response:

```
{"success": true, "distance": number}
```

`distance` should be in meters. Below is the expected output for each example session file:

* `session36255.json` distance = 1415.1 meters
* `session35935.json` distance = 2800.4 meters
* `session36261.json` distance = 8206.7 meters

Upon failure, the endpoint should give this response:

```
{"success": false, "message": string}
```

Again, try to catch as many error cases as you can within reason.


