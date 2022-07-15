---
title: "Building the Data Action"
chapter: false
weight: 40
---


At this point you've identified your API call and have parsed it for the data you're trying to retrieve, now we need to tie it all together into a Data Action that can be used within Architect flows or scripts.

We will start by Navigating to Admin > Actions and creating a new data action under the integration type that you created.

![image](/images/createaction.PNG)

## Contracts
The first step is constructing our input contract, which as we know from the developer center, only requires the Queue ID as input. By naming your first object you can add your input string of QueueID underneath it by selecting the "+" and ensuring the field is set to type string.

![image](/images/DAinputcontract.PNG)

You will then define your output contract, while there are various ways to construct output contracts to allow for additional data, you will construct an object that is an array of strings.

![image](/images/DAoutputcontract.PNG)

## Configuration
Within the configuration tab we will begin by defining our Request URL Template using the API URL we've gathered from the developer center, in this case it will look like -
```
/api/v2/routing/queues/INSERTQUEUEGUIDHERE/members?expand=presence
```
We will need to use our available inputs to replace the static queue GUID with the input variable we created of QueueID. You can do this by highlighting the Queue GUID and clicking the "QueueID (String)" Input on the right side of the screen.

![image](/images/DAconfiginput.PNG)

The response field is where we will construct our translation map to parse the response, translation map defaults to gracefully handle empty responses and success templates to map our translation to our output variable. Below is a code example of a response configuration.

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

Within the translation map section we have a Key Value Pair of Presence, and the path to the array of user presences we discovered previously. 

The Success template is configured to map the value from the 'Presence' Key Value Pair, to the output array we constructed in the contract area.

We can now test this configuration to ensure the data action is functioning!

## Testing

By manually inputing a QueueID we can perform a test against our configuration to ensure the data is returning as expected.
In the image below we can see a successful execution with our presence values properly mapping to our output array.

![image](/images/DAtest.PNG)

If we were to have made a typo, or error in the configuration, the test would return the error and step in the execution process to assist us in troubleshooting. In the examples below I have placed a typo in the value of my presence key value pair, creating a failure at step 10 in the REST process.

![image](/images/DAtestfailure1.PNG)

By navigating down to step 10 we can see a breakdown of the actual error
```
{
  "message": "Transform failed to process result using 'successTemplate' template due to error:'Variable $Presences has not been set at successTemplate[line 1, column 20]'\n Template:'{\"PresenceArray\" : $Presences}'.",
  "code": "bad.request",
  "status": 400,
  "messageParams": {},
  "contextId": "fc7c43df-fb93-4651-83b2-2dc942e623d7",
  "details": [
    {
      "errorCode": "ACTION.PROCESSING"
    }
  ],
  "errors": []
}
```
This example states "$Presences has not been set at the successtemplate", helping me identify that I have a mismatch between my translation map and success template. In the example below, line 8 has plural presences, whereas line 3 has singular.

![image](/images/DAtestfailure2.PNG)

When tests pass and perform as expected, you can publish your data action for use within flows or scripts!