---
title: "Building the Data Action"
chapter: false
weight: 30
---

## Building the Data Action
At this point you've identified your API call and have parsed it for the data you're trying to retrieve, now we need to tie it all together into a Data Action that can be used within Architect flows or scripts.

You will start by constructing our input contract, which as we know from the developer center, only requires the Queue ID as input. By naming your first object you can add your input string of QueueID underneath it by selecting the "+" and ensuring the field is set to type string

![image](/images/DAinputcontract.PNG)

You will then define our output contract, while there are various ways to construct output contracts to allow for additional data, you will construct an object that is an array of strings.

![image](/images/DAoutputcontract.PNG)

Within the configuration tab we will begin by defining our Request URL Template using the API URL we've gathered from the developer center, in this case it will look like -
```
/api/v2/routing/queues/INSERTQUEUEGUIDHERE/members?expand=presence
```
We will need to use our available inputs to replace the static queue GUID with the input variable we created of QueueID. You can do this by highlighting the Queue GUID and clicking the "QueueID (String)" Input on the right side of the screen

![image](/images/DAconfiginput.PNG)

The response field is where we will construct our translation map to parse the response, translation map defaults to gracefully handle empty responses and success templates to map our translation to our output variable. Below is a code example of a response configuration

```
{
  "translationMap": {
    "Presence": "$.entities[*].user.presence.presenceDefinition.systemPresence"
  },
  "translationMapDefaults": {
    "Presence": "null"
  },
  "successTemplate": "{\"PresenceArray\" : $Presence}"
}
```




### Objects and Arrays

![image](/images/objectsarrays.PNG)
