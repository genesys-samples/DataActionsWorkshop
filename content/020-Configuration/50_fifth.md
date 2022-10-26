---
title: "Data Actions within Flows"
chapter: false
weight: 50
---


With our Data Action tested and validated, the last step is to actually use it! For this section we will create a basic inbound call flow that calls our data action by navigating to Admin > Architect > Inbound Call Flows > Create new.

![image](/images/ArchitectFlowCreate.PNG)

Within our flow we will create a reusable task, these are the 'blank canvas' of routing and allow for more advanced logic than a standard menu. Select Add reusable task here > Toolbox > Task.

![image](/images/architectreusable.PNG)

Select the 3 dots next to the task and assign this as the starting task. This means all calls entering the flow will begin with this task.

![image](/images/architectsetstart.PNG)

From the toolbox, we will add the 5 following items in this order -

1. Call Data Action - At the very start
2. Decision - Within the success path of the data action
3. Transfer to ACD - Within the **Yes** path of the Decision
4. Transfer to Voicemail - Within the **No** path of the Decision
5. A disconnect at the very bottom of the flow

When done, your flow should look like the image below -

![image](/images/architectflowoutline.PNG)

We will now configure each of these elements. Within your "Call Data Action" Select your integration type (likely "Genesys Cloud Data Actions") and find your data action by name.

We will provide the queue ID to the data action by changing the QueueID Input to "Literal" and pasting in our queue GUID.
The last item is to set the output to a flow variable for reference within our decision, for instance **Flow.presences**. When completed your Data Action configuration should look like - 

![image](/images/architectdataaction.PNG)

The next item to configure is the **Decision**, which is where we will apply logic against the returned list of of queue user presences. Change the expression from boolean, to expression -

![image](/images/architectdecision.PNG)

Paste the following code snippit into the expression field - 

```
Contains(ToString(Flow.presences), "On Queue")
```

This a very simplistic logic block that converts our Presence Array into a single string and checks to see if there are any agents showing as **On Queue**.

We will now point the Transfer to ACD and Transfer to Voicemail to the queue used in the data action lookup. After setting these your flow is ready to publish and should look like - 

![image](/images/architectfinalflow.PNG)

You can test this logic by going on-queue or off queue and calling this flow.