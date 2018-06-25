# Instructions On Running The RVChallange API
Below is a list of available exposed api methods. Identifying ID's that would represent the user & visit was removed from the URL. These values were put into the body so they would not be exposed. the UserId is placed in the header and is being used to verify that they user does exists in the system. This check is being done per request in ValidateRequest.cs. 

The api is currently being hosted at http://rvchallenge.azurewebsites.net

- api/user/valid
- api/user/visit/delete
- api/user/visits/distance/{distanceType}
- api/user/visit/insert
- api/user/visits/cities
- api/user/visits/states
- api/state/{stateId}/cities
- api/state

The database being used is SQL Server hosted in Azure. A couple of options can be used to handle large data requests from the user. One is to limit the number of records to come back in the query or second, use SELECT ROW_NUMBER() to determine the start and end records to return.

For every response that is being sent back to the user, a Message object is returned. This message will have the IsSuccess, SystemMessage and ApplicationFriendlyMessage properties. If the request is successful, IsSuccess will be equal to 1, otherwise 0. If an error has occurred, SystemMessage will be populated with the error. ApplicationFriendlyMessage can be used to display messages that will be used for the user interface.

A user is able to add a visit to the same city multiple times. The DateVisited will be updated with the date the city was visited.

The database schema files will be uploaded and called RVChallenge table scripts.sql & RVChallenge stored procedure scripts.sql.

Users that are available are located in "RVChallenge user list.csv"

## api/user/valid
This method will validate the user. It determines if the user is in the system or is active within the system.

**Fiddler example**

**Request Type:** GET

**URL:** http://rvchallenge.azurewebsites.net/api/user/valid

**Header:**

```
User-Agent: Fiddler
Host: localhost:59943
UserId: 1
```

**Result**

```
{
  "UserId": 1,
  "Message": {
    "ApplicationFriendlyMessage": null,
    "SystemMessage": null,
    "IsSuccess": true
  }
}
```

## api/user/visit/delete
This will delete a city that a user has visited.

**Fiddler example**

**Request Type:** POST

**URL:** http://rvchallenge.azurewebsites.net/api/user/visit/delete

**Header:**

```
User-Agent: Fiddler
Host: localhost:59943
Content-Length: 21
Content-Type: application/json; charset=utf-8
UserId: 1

```

**Request Body:**

```
{
  "visitId": "8"
}
```

**Result**

```
{
  "UserId": 1,
  "Message": {
    "ApplicationFriendlyMessage": null,
    "SystemMessage": null,
    "IsSuccess": true
  }
}
```

**Result (Error ex.)**

```
{
  "UserId": 1,
  "Message": {
    "ApplicationFriendlyMessage": null,
    "SystemMessage": "Visit does not exists. Could not delete.",
    "IsSuccess": false
  }
}
```

## api/user/visits/distance/{distanceType}
This method will return the total distance traveled by the user from all of their visits. DistanceType can be set to 1=Kilometers, 2=Meters or 3=Miles.

**Fiddler example**

**Request Type:** GET

**URL:** http://rvchallenge.azurewebsites.net/api/user/visits/distance/3

**Header:**

```
User-Agent: Fiddler
Host: localhost:59943
UserId: 1
```

**Result:**

```
{
  "Visits": null,
  "TotalDistanceTraveled": 328.82575146977325,
  "UserId": 1,
  "Message": {
    "ApplicationFriendlyMessage": null,
    "SystemMessage": null,
    "IsSuccess": false
  }
}
```

## api/user/visit/insert
Inserts a visit to a city that a user has traveled to.

**Fiddler example**

**Request Type:** POST

**URL:** http://rvchallenge.azurewebsites.net/api/user/visit/insert

**Header:**

```
User-Agent: Fiddler
Host: localhost:59943
Content-Length: 22
Content-Type: application/json; charset=utf-8
UserId: 1

```

**Request Body:**

```
{
   "cityId": "321"
}
```

**Result**

```
{
  "UserId": 1,
  "Message": {
    "ApplicationFriendlyMessage": null,
    "SystemMessage": null,
    "IsSuccess": true
  }
}
```

**Result (Error ex.)**

```
{
  "UserId": 1,
  "Message": {
    "ApplicationFriendlyMessage": null,
    "SystemMessage": "City does not exist. Could not insert visit.",
    "IsSuccess": false
  }
}
```

## api/user/visits/cities
Get a list of all the cities the user has visited.

**Fiddler example**

**Request Type:** GET

**URL:** http://rvchallenge.azurewebsites.net/api/user/visits/cities

**Header:**

```
User-Agent: Fiddler
Host: localhost:59943
UserId: 1
```

**Result**

```
{
  "Visits": [
    {
      "VisitId": 9,
      "UserId": 1,
      "CityId": 112,
      "DateVisited": "2018-05-16T12:18:27.213",
      "DateAdded": "2018-05-16T12:18:27.213",
      "LastUpdated": "0001-01-01T00:00:00",
      "City": {
        "CityId": 112,
        "Name": "Hazel Green",
        "StateId": 1,
        "StateName": "Alabama",
        "Status": "verified",
        "Latitude": 34.928785,
        "Longitude": -86.571848,
        "DateAdded": "2018-05-16T12:18:27.213",
        "LastUpdated": "0001-01-01T00:00:00"
      }
    },
    {
      "VisitId": 10,
      "UserId": 1,
      "CityId": 321,
      "DateVisited": "2018-05-16T12:59:18.93",
      "DateAdded": "2018-05-16T12:59:18.93",
      "LastUpdated": "0001-01-01T00:00:00",
      "City": {
        "CityId": 321,
        "Name": "Kokhanok",
        "StateId": 2,
        "StateName": "Alaska",
        "Status": "verified",
        "Latitude": 59.438042,
        "Longitude": -154.773819,
        "DateAdded": "2018-05-16T12:59:18.93",
        "LastUpdated": "0001-01-01T00:00:00"
      }
    },
    {
      "VisitId": 6,
      "UserId": 1,
      "CityId": 215,
      "DateVisited": "2018-05-15T13:33:32.3",
      "DateAdded": "2018-05-15T13:33:32.3",
      "LastUpdated": "0001-01-01T00:00:00",
      "City": {
        "CityId": 215,
        "Name": "Trinity",
        "StateId": 1,
        "StateName": "Alabama",
        "Status": "verified",
        "Latitude": 34.60617,
        "Longitude": -87.087974,
        "DateAdded": "2018-05-15T13:33:32.3",
        "LastUpdated": "0001-01-01T00:00:00"
      }
    }
  ],
  "TotalDistanceTraveled": 0,
  "UserId": 1,
  "Message": {
    "ApplicationFriendlyMessage": null,
    "SystemMessage": null,
    "IsSuccess": true
  }
}
```

## api/user/visits/states
Get a list of all the states the user has visited.

**Fiddler example**

**Request Type:** GET

**URL:** http://rvchallenge.azurewebsites.net/api/user/visits/states

**Header:**

```
User-Agent: Fiddler
Host: localhost:59943
UserId: 1
```

**Result**

```
{
  "States": [
    {
      "StateId": 1,
      "Name": "Alabama",
      "Abbreviation": "AL",
      "DateAdded": "0001-01-01T00:00:00",
      "LastUpdated": "0001-01-01T00:00:00"
    },
    {
      "StateId": 2,
      "Name": "Alaska",
      "Abbreviation": "AK",
      "DateAdded": "0001-01-01T00:00:00",
      "LastUpdated": "0001-01-01T00:00:00"
    }
  ],
  "UserId": 1,
  "Message": {
    "ApplicationFriendlyMessage": null,
    "SystemMessage": null,
    "IsSuccess": true
  }
}
```

## api/state/{stateId}/cities
Get a list of all the cities in a given state.
**Fiddler example**

**Request Type:** GET

**URL:** http://rvchallenge.azurewebsites.net/api/state/3/cities

**Header:**

```
User-Agent: Fiddler
Host: localhost:59943
UserId: 1
```

**Result**

```
{
  "Cities": [
    {
      "CityId": 417,
      "Name": "Apache Junction",
      "StateId": 3,
      "StateName": "Arizona",
      "Status": "verified",
      "Latitude": 33.42314,
      "Longitude": -111.546119,
      "DateAdded": "2015-03-01T00:00:00",
      "LastUpdated": "2015-03-01T00:00:00"
    },
    {
      "CityId": 419,
      "Name": "Ash Fork",
      "StateId": 3,
      "StateName": "Arizona",
      "Status": "verified",
      "Latitude": 35.2239,
      "Longitude": -112.481424,
      "DateAdded": "2015-03-01T00:00:00",
      "LastUpdated": "2015-03-01T00:00:00"
    },
    {
      "CityId": 412,
      "Name": "Avondale",
      "StateId": 3,
      "StateName": "Arizona",
      "Status": "verified",
      "Latitude": 33.4405,
      "Longitude": -112.349664,
      "DateAdded": "2015-03-01T00:00:00",
      "LastUpdated": "2015-03-01T00:00:00"
    },
    ...
```

## api/state/
Get a list of all the states.
**Fiddler example**

**Request Type:** GET

**URL:** http://rvchallenge.azurewebsites.net/api/state

**Header:**

```
User-Agent: Fiddler
Host: localhost:59943
UserId: 1
```

**Result**

```
{
  "States": [
    {
      "StateId": 1,
      "Name": "Alabama",
      "Abbreviation": "AL",
      "DateAdded": "2015-03-01T00:00:00",
      "LastUpdated": "2015-03-01T00:00:00"
    },
    {
      "StateId": 2,
      "Name": "Alaska",
      "Abbreviation": "AK",
      "DateAdded": "2015-03-01T00:00:00",
      "LastUpdated": "2015-03-01T00:00:00"
    },
    {
      "StateId": 3,
      "Name": "Arizona",
      "Abbreviation": "AZ",
      "DateAdded": "2015-03-01T00:00:00",
      "LastUpdated": "2015-03-01T00:00:00"
    },
    {
      "StateId": 4,
      "Name": "Arkansas",
      "Abbreviation": "AR",
      "DateAdded": "2015-03-01T00:00:00",
      "LastUpdated": "2015-03-01T00:00:00"
    },
    ...
```



# RV Coding Challenge

A new client wants to build a small API to allow users to pin areas they've visited and potentially share them with other users. The client included a set of sample data in `User.csv`, `City.csv`, and `State.csv`. Please implement a few basic operations on the data provided, including the following.

 - Listing the cities in a given state
 - Registering a visit to a particular city by a user
 - Removing a visit to a city
 - Listing cities visited by a user
 - Listing states visited by a user.  

You may use whatever language or tools you wish to complete the exercise.  Keep in mind that you may be asked to extend your solution in an on-site interview.


**Required endpoints**

1. List all cities in a state

	`GET /state/{state}/cities`
 
2. Allow to create rows of data to indicate they have visited a particular city.

	`POST /user/{user}/visits`

	```
	{
		"city": "Chicago",
		"state": "IL"
	}
	```
	
3. Allow a user to remove an improperly pinned visit.

	`DEL /user/{user}/visit/{visit}`

4. Return a list of cities the user has visited

	`GET /user/{user}/visits`
	
5. Return a list of states the user has visited

	`GET /user/{user}/visits/states`


## Things To Consider

- How should you deal with invalid or improperly formed requests?
- How should you handle requests that result in large data sets?


## Deliverables

- The source code for your solution.
- The database schema you use to implement your solution.
- Any additional documentation you feel is necessary to explain how your application works, or describe your thought process and design decisions.


## Bonus Points

- Handle authentication of users prior to allowing changes to their visits
- Make use of the lat/long data for cities in a creative way that provides additional functionality for the client



