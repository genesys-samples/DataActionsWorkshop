---
title: "Component Definitions"
chapter: false
weight: 20
---

## Data Action Components

Data Actions can be broken into the 4 following components

**1. Input Contracts**

**2. Output Contracts**

**3. Request URLs**

**4. Response Bodies**

### Input Contracts
Input Contracts are variables, or arrays of variables that we supply in the REST call to tell the API what specific piece of data we’re trying to invoke.
  * Example: We’re trying to retrieve user presence data from a specific queue, we construct a QueueID Variable to input into our API call allowing us to dynamically provide a new Queue ID on every call.

> ***Not all API Calls require a defined Input Contract, and inputs can be statically assigned.***

In the image below we have created an object with a QueueID input string, this input is then assigned in the request URL to allow for a dynamic Queue ID value to be assigned from an architect flow. Alternatively, if the Queue ID value does not need to change, it can be statically assigned.

![image](/images/inputcontracts.PNG)

### Output Contracts
In an API response we may receive hundreds of lines of data, but only care about a single line.
Output Contracts are where we define what data from the call we want to action off of.
  * Example: We’ve executed a call to provide us all of the queue details, we only need to see agent presence, so we set a variable of Presence to store this information at the end of the call.

  >**Not all API Calls require a defined Output Contract; Some calls may not actually return data that we wish to reference.**

In the image below we've constructed our Output Contract and have only constructed Presence as an output variable. In the response body defined later we can parse the response for presence and store just that value to our single output variable.

![image](/images/outputcontracts.PNG)

### Request URLs
Request URL’s are the actual call that is being executed and contain the specific API path that we are invoking. In our Data Action Constructor some calls can be defined in the simple view where we define just the URL and inputs, while other calls may require additional predicates and to be formatted in the JSON Field.

In the image below we can see both the **Simple** and **JSON** request templates. We can also see the QueueID Input variable assigned in both request formats.

![image](/images/requesturls.PNG)

### Response Bodies

The Response Body defines what information from the call we are trying to return and can be broken into 3 components - 
  * Translation Maps allow us to parse and map data from the API Response to variables.
  * Translation Map Defaults allow us to define  default values in the event no information is returned.
  * Success Templates allow us to map the parsed data to the output contract variables we constructed so the data can be utilized for routing, scripting, etc.

In the image below we have -
1. Constructed a translation map with a key value pair of 'Presence' and the path used to parse the API response for only presence information.
2. Defined a translation map to apply a default value to 'Presence' of "NOT_SET" in the event that we do not return any data. This will allow the request to gracefully fail if no response is returned.
3. Mapped the value of 'Presence' from our translation map, to our *<ins>'Presences'<ins>* output variable.

![image](/images/responsebodies.PNG)

### Data Action Test Tool
The Data Action Constructor provides a test tool which allows you to test calls and validate that they are configured properly prior to deploying them. The test tool also contains an operations sequence to show you where in the RESTful process that the call is failing to allow for easier troubleshooting.

In the image below we can see that the test failed to apply the output transformation, indicating the translation map should be the starting point for our troubleshooting.

![image](/images/testtool.PNG)