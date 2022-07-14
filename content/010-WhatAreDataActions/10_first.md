---
title: "Basic Definitions"
chapter: false
weight: 10
---

## Defining Data Actions
Simply put, data actions are what Genesys defines as our REST API call constructor.

Data actions are used to access information both internal and external to the platform that wouldn't otherwise be available. This information, such as advanced queue or CRM data can be used to invoke powerful routing decisions or provide advanced script/screenpop information to your agents (amongst many other utilizations). Data actions can be split into 2 categories, **Internal** and **External**.

**Internal Data Actions** are ‘typically’ used to make routing decisions or update platform resources and configurations.
  * Example: Checking how many agents are on queue to determine whether to offer the caller a callback option.

**External Data Actions** can be used to pull information from external systems to populate scripts, or update external systems with information the agent gathered on the call.
 * Example: Pulling a customer record from an external CRM to display account information to the agent within the script, and updating that customer record from within the script.

