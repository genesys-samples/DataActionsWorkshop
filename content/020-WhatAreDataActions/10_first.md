---
title: "First Lesson"
chapter: false
weight: 10
---

## Defining Data Actions
Simply put, Data Actions are what Genesys defines as our REST API call constructor. Data Actions are used to construct API and can be split into 2 categories, **Internal** and **External**

**Internal Data Actions** are ‘typically’ used to make routing decisions or update platform resources and configurations
  * Example: Checking how many agents are on queue to determine whether to offer the caller a callback option

**External Data Actions** can be used to pull information from external systems to populate scripts, or update external systems with information the agent gathered on the call
 * Example: Pulling a customer record from an external CRM to display account information to the agent within the script, and updating that customer record from within the script

