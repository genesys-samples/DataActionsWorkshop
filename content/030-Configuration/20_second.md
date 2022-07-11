---
title: "Parsing API Responses"
chapter: false
weight: 20
---

## Understand JSON Paths
JSON responses are series of objects and arrays in a hierarchical structure. You start with a base object, in the below example the base object will be a store. The example store sells an array of different foods and silverware. If we run a query against the stores inventory we will recieve a response containing all of the objects that the store sells and any attributes associated to these objects, such as their category (fruits, vegetables etc.) and price.

![image](/images/storeexample.PNG)

Now what if we only care about what fruits the store sells? or we're budget shopping and only want to see what the store has for under $2.00? Defining a JSON Path against the response will allow us to isolate only the items that we're concerned about.

An example of a JSON Path to find all of food the store offers would look like - 
```
$.Thestore.food[*]
```
**'$'** - Says to start at the root object of "Thestore"
**'.food'** - Says to step into the next object or array within the store of "food"
**'[*]'** - is an array position notation, stating to return all objects within the food array, this could be changed to '[0]' to return only the first object within this array

An example of a path that only returns objects that are under $2.50 - 
```
$.Thestore.food[?(@.price<2.50)]
```
The only difference here is we are no longer defining an array position and have now applied a filter **' [?(@.price<2.50)] '** that looks at the price key and filters for only objects that are less than 2.50

## Parsing the Response Body

Now that you've found an API call that returns the data that we need, we need to determine the path that will return only the presence information we are looking for.
>As you saw in the response on the developer center, much like our full store query example above, this specific call returned many lines of data that are irrelevant to the queue presence check, and depending on the specific call, or how many members in the queue, you may have returned thousands of lines of data. 

There are many external tools that can assist you in parsing through JSON responses for the data you need. While Genesys does not have preferred tools for performing these actions, below are a few publicly accessible tools that will be used to perform these actions for this workshop. 
>These tools are not required but can assist while learning JSON parsing.

https://goessner.net/articles/JsonPath/index.html#e2 - Can provide definitions of JSON functions and parsing examples

https://jsonpathfinder.com/ - Can assist with finding paths to specific objects within JSON responses

http://jsonpath.com/ - Can provide visual representations of what data will be returned from specific paths

By pasting the response from our developer center into tools such as path finder, you can see a visual breakdown/hierarchy of the objects and arrays within the JSON response.

In the Example below, we can see the path finder tool has identified 2 entities within my JSON response, which correlate to the 2 users I have in the queue I've constructed.

![image](/images/pathfinder1.PNG)

By expanding fields underneath one of the entities, we can eventually find the presence field. By selecting the presence field, we see that the tool has populated a "Path" to this field for us. 

![image](/images/pathfinder2.PNG)

Now that we have our path, it's important to visualize the data and filter or expand our path where needed. By pasting the full response from the developer center into the JSONPath Input field, and pasting the path we discovered from JSONPathfinder into the path field, we can visualize what the actual path evaluates out to.
>you will need to replace the "X" that JSONPathFinder places at the start of the path with a "$"

![image](/images/Jsonpath1.PNG)

The response here shows a single offline status, however; we know that there are multiple users and we need the presence information for all of them. The path we are using has an array position of "[0]", meaning it's only returning the presence of the first entity, or user, within the response. By changing this to a wildcard - "[*]", we can return the presence information for all users.

![image](/images/Jsonpath2.PNG)