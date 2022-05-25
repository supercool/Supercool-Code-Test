# Supercool code test (May 2022)

## Introduction

Thanks for agreeing to run through this exercise.

During it you'll be tackling a task that is based on requesting data from a JSON endpoint and transforming it into a more useful structure.

This is intended to simulate part of the real life functionality involved in integrating with 3rd party CRM's that our core events management craft plugin is based around.

You should be able to finish the exercise in about an hour, please don't spend too much longer than that in total on this: it's fine to leave parts out. The aim _isn't_ for you to produce perfect, production ready code, it's for you to demonstrate how you approach a new project/problem. We're interested more in your thinking and general approach than the final output.

We'd like you to code in PHP, please also initialise a git repository (local or on github) to commit your code to.

### Tips
* You might find it useful to keep notes on your thinking as we'll be asking you to talk us through your approach during the next call
* You're welcome to use any (or no) frameworks / helper libraries you'd like
* We're not expecting you to write tests for your code, but if you find it easier to follow a TDD approach you're welcome to


## The task

We have set up 3 JSON endpoints that contain some dummy data for "events", "instances" and "venues". In terms of a pantomime:
* An "event" represents the entire production - Url: [https://supercooldesign.co.uk/api/technical-test/events](https://supercooldesign.co.uk/api/technical-test/events)
* An "instance" represents a specific performance of the pantomime - Url: [https://supercooldesign.co.uk/api/technical-test/instances](https://supercooldesign.co.uk/api/technical-test/instances)
* A "venue" is the place a specific instance is taking place - Url: [https://supercooldesign.co.uk/api/technical-test/venues](https://supercooldesign.co.uk/api/technical-test/venues)

Your overall task will be write a program that will request the data from these 3 endpoints, relate events to instances and instances to venues, and then process the data in order to produce an ordered list of events at each venue.

### Step 1 - Requesting data

Your will first need to send GET requests to the 3 urls listed above, in order to retrieve the data to work with.

Safe Assumption 1: it is fine to assume that the urls will never error, and will always return JSON in the following formats:

Events, an array of event objects:
```json
[
  {
    "id": "12345ABCDEFG",
    "startSelling": "2021-01-01T00:00:00",
    "stopSelling": "2024-01-01T00:00:00",
    "title": "Event 1"
  }
]
```

Venues, an array of venue objects:
```json
[
  {
    "id": "34567ABCDEFG",
    "title": "Venue 1"
  }
]
```

Instances, an array of instance objects, where the ID's inside the "event" and "venue" objects unique define which event this is an instance of, and which venue it is at respectively:
```json
[
  {
    "id": "67890ABCDEFG",
    "start": "2023-01-01T10:30:00",
    "event": {
      "id": "12345ABCDEFG"
    },
    "venue": {
      "id": "34567ABCDEFG"
    },
    "attribute_audioDescribed": true
  }
]
```

Safe Assumption 2: it is also fine to assume that the event and venue id's on the instance object will be valid, and correspond to an object in the relevant API response


### Step 2 - Data processing and output

We want to produce a list of venues with events on sale.

Using the data you gathered in step 1, write some code that create a simple HTML file containing a list of venues with events that are currently on sale. Save this HTML to disk.

* An event is on sale if the "startSelling" date has passed, but the "stopSelling" date hasn't. If this is not the case, exclude the event
* If a venue has no on sale events, exclude it
* Venues should be in alphabetical order
* Events at venues should be ordered by their first instance date (the earliest related instance "start" date)
* The events should contain the following information: 
    * title
    * id: the numeric part of the event id (i.e "12345" part of "12345ABCDEFG")
    * first instance: The earliest "start" date of any related instance
    * next instance: The earliest "start" date of the any related instance in the future
    * last instance: the latest "start" date of any related instance
    * instance count: the total number of related instances
    * audio described instance count: the total number of related instances that have their "attribute_audioDescribed" json key as true
* Safe assumption 3: you can assume all instances for an event will be at the same venue
* The HTML does not need to be styled

Example output:

```html
<ul>
    <li>
        Venue 1:
        <ul>
            <li>
                Event 3: <br>
                id: 12345 <br>
                First instance: 2022-01-01T10:30:00 <br>
                Next instance: 2022-07-05T11:00:00 <br>
                Last instance: 2022-07-05T11:00:00 <br>
                Instance count: 5 <br>
                Audio Described instance count: 2
            </li>
            <li>
                Event 1: <br>
                id: 45657 <br>
                First instance: 2023-01-01T10:30:00 <br>
                ...
            </li>
        </ul>
    </li>
    <li>
        Venue 3:
        <ul>
            ...
        </ul>
    </li>
</ul>
```


### Step 3 - Send us your code
Commit your code to github and make it public and send us the URL, or send us a zip of the project folder.


### For discussion later
* How would your code need to change if the listed "safe assumptions" could no longer be assumed?
    * If Api endpoints can now error or timeout?
    * Instances "event" and "venue" id's are no longer guaranteed to correspond to an an object in the respective API response (e.g what if a venue is set to "invisible" in the api)?
    * An events instance can now occur at different venues?
* If, rather than generating HTML, your script generated JSON data for an API response that could potentially be requested several times a second, how would you prevent it from crashing its server?



