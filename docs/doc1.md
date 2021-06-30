---
id: doc1
title: Inspera Assessment API
sidebar_label: Inspera Assessment API
slug: /
---


:::note

This document provides general documentation on the Inspera APIs and a quick user guide on getting started. If you have any questions, requests or need any assistance, feel free to get in touch via api@inspera.no or your dedicated contact.

Are you looking for general information on integrations with Inspera Assessment? If so, take a look at APIs and Integrations.

:::


## General information
The Inspera Assessment API is a set of open REST APIs allowing external systems to access a subset of Inspera Assessment functionality using Inspera Assessment user privileges. The functionality provided will include creating and updating tests, assigning and removing learners and contributors from tests, exporting results and responses after grading, as well as user management. 

We develop and update and add new APIs continuously. All changes and additions to existing APIs, as well as new developed APIs and API roadmap plans are announced through Inspera's release notes. Please see our product site to learn about how to subscribe to release notes here: https://support.inspera.com/hc/en-us/articles/360038889831-Product-updates-release-notes-2020


### Inspera APIs in customer landscape
Illustration of how APIs fit into customer system landscape (APIs and integration in general are presented as blue blocks)

![img](../static/img/Skjermbilde 2020-10-11 kl. 22.10.10.png)

Inspera's APIs are build as groups of APIs, depending on the functions and modules in Inspera Assessment they touch upon:

1. **Candidate API group** (handle export of learner submissions and results export)
- Export of learners submissions (PDF)
- Export of learners results (JSON)
2. **Test API group** (APIs to transfer test events to Inspera Assessment)
- Create or update tests
- Assign or remove learners and contributors
- Export of test materials (PDF / QTI)
- Export of test metadata
- Register or redraw appeals
- Set or change test settings NEW
- Register or export explanations
- Search tests based on criteria
3. **Users API group** (transfer users from external systems to Inspera Assessment)
- Create, update or delete administrative users
- Create or update learners (students)
- Search for users
- Export user information

We also provide set of Webhooks and events to make  machine - to - machine integrations possible. Inspera provides a set of events that systems can subscribe to and act accordingly to make workflows between systems possible. We update and add new events continuously, full overview over available events can be obtained from GET action on the following: https://ia.inspera.no/apidoc/#/webhooks_(beta)/listAllSubscriptions 

## Getting started
### First steps
To get started, you will need a user in Inspera Assessment, with Administrator privileges. From the user profile interface, you will then be able to access Authorization details for either yourself or other Inspera users in your organization. These authorization details will include an authentication code. In addition to this code,  client id is needed to be able to send API call to obtain authorization token. This will be explained later in this document.

The authentication code will be accessible only once and if a user loses or forgets their authorization code, any administrator for your organization will be able to revoke that user's access and generate a new code for them.



::: Important note
Users whose API authorization code will be used to make API calls must have "extended access" privileges.

We strongly recommend to create dedicated Integration user on your Inspera Assessment system, with only "extended access" privilege, and use API authorization code from this user for integration API calls. This is to make integration user-independent. 
:::

Where is API key in Inspera Assessment user interface?

Open "User administration" from Inspera Assessment main page and search for user you want to generate key for. The following will appear on the screen:

pic 1
pic2 
pic 3

How to obtain client_ID?
"client_ID" is a parameter needed for the correct access token to be generated. This value is, in most cases, host-part of the domain of your Inspera Assessment url. For instance, if your Inspera URL is https://test.inspera.com/ , clientID will be "test".  However, this may not be the case for all clients, in which case, please contact us at api@inspera.com.

Once a user has their authorization code and have obtained client id, they will be able to start using the APIs.

Update release December 2020:

ClientID is now visible in Inspera Assessment together with API key generation field:

pic clientid 

Authentication and access tokens

An authentication code and client id are not directly used to access the APIs - a user will first have to call the Authenticate API (see below) and acquire a short-lived access token. All of the actual API calls will then be made using this token. Once a token expires, users will need to acquire a new one, using either the original authentication code, or using a refresh token provided by the Authenticate API (refresh tokens are not implemented at the moment).

