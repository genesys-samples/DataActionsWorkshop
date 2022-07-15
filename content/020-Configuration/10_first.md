---
title: "Developer Center"
chapter: false
weight: 10
---

> **The Genesys Cloud Developer center contains numerous developer resources such as code examples, blueprints, SDK's, and as we will see shortly - A robust API explorer.**





## Identify and Execute your API Call

In this section we will be finding and executing a call to find the presence of all users within a given queue.

Once you have navigated to https://developer.genesys.cloud/ and logged into the developer center, select the API Explorer tile displayed below.

![image](/images/devcenter.PNG)

The API Explorer allows you to filter and search through all accessible API Endpoints for the call you're looking for and execute that call without requiring a fully configured data action.

We will start by filtering for the API's we think may contain the information we're looking for.
>**You may need to work through API calls you believe are relevant and execute them to determine which endpoint provides the information you need. There may also be numerous calls that provide the piece of data you are looking for. In this workshop we will navigate directly to the call that we need.**

1. On the right hand side, clear all categories and then add the routing category back to the filter.
2. Search for "Queue" in the search box
3. Check the "GET" box to filter for only GET requests

![image](/images/explorerfilter.PNG)

Once we have found our call, in this case **/api/v2/routing/queues/{queueId}/members** we need to configure it prior to execution.

1. disable reading mode, this will allow us to execute directly from the developer center
2. input your queueId
>**Most configuration components within Genesys Cloud are assigned a GUID, or Globally Unique Identifier. By navigating to the queue from your administration panel you can find the GUID within the URL.**

![image](/images/queueguid.PNG)

3. Expand the call to display Presence information
>**Some calls hide additional information by default, but can be expanded or filtered to add or remove information.**

![image](/images/explorerconfig.PNG)

Now that our call has been configured, we will execute the call and verify the information we're looking for is contained within the response body.
>**We know we've found the correct call because we can see user presence in the response body.**

![image](/images/explorerexecute.PNG)

