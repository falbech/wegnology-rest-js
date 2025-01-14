# WEGnology Javascript REST API Client

The [WEGnology](https://docs.app.wnology.io/getting-started/what-is-wegnology/) REST API client provides a simple way to use the comprehensive WEGnology API. You can authenticate either as a WEGnology device or with your user account, and have access to all the functionality of the WEGnology platform.

This client works both in browsers and node.js (v0.12 and newer), and supports both callbacks and promises. It uses [Axios](https://github.com/mzabriskie/axios) under the covers for the actual HTTP communication.

<br/>

## Installation

The latest stable version is available in NPM and can be installed using

```bash
npm install --save wegnology-rest
```

<br/>

## Example

Below is a high-level example of using the WEGnology JavaScript REST API client to authenticate against the WEGnology Platform and report state for a device.

```javascript
var api = require('wegnology-rest');
var mySensor = require('./my-analog-sensor');

var client = api.createClient();

client.auth.authenticateDevice({ credentials: {
  deviceId: 'my-device-id',
  key: 'my-app-access-key',
  secret: 'my-app-access-secret'
}}).then(function (response) {
  client.setOption('accessToken', response.token);
  var appId = response.applicationId;

  var state = { data: { temperature: mySensor.read() } };
  return client.device.sendState({
    deviceId: 'my-device-id',
    applicationId: appId,
    deviceState: state
  });
})
.then(function (response) {
  console.log(response); // { success: true }
})
.catch(function (error) {
  console.error(error);
});
```

<br/>

## API Documentation

```javascript
var client = api.createClient({ accessToken: undefined, url: 'https://api.app.wnology.io' })
```

The wegnology-rest module exposes a single function named createClient which
returns a client instance. It takes the following options:

* accessToken  
  The access token to be used for authentication - by default there is no access token. An access token can be acquired through any of the [Auth](docs/auth.md) methods, or can be created for a particular application through [applicationApiTokens](docs/applicationApiTokens.md).

* url  
  The url of the WEGnology API - by default <https://api.app.wnology.io>.

### Client

A client is a single api instance. By default, it is unauthenticated, but can be given an access token to perform authenticated requests.

#### Functions

* setOption(name, value)  
  The setOption function can be used to change any of the options currently set on the client, such as the access token.

* getOption(name)  
  The getOption function can be used to retrieve the current value of any of the options set on the client instance.

#### Resources

Each of the following is a property on the client instance, and wraps the actions for that particular resource. See each resource documentation file for more information.

* [application](docs/application.md)  
  Contains all of the actions that can be performed against a single [Application](https://docs.app.wnology.io/applications/overview/), which include things like getting info on an application or modifying an application.

* [applications](docs/applications.md)  
  Contains all of the actions that can be performed against the set of [Applications](https://docs.app.wnology.io/applications/overview/) that the currently authenticated user has access to - such as listing the applications or creating a new application.

* [applicationApiToken](docs/applicationApiToken.md)  
  Contains all the actions that can be performed against a single [Api Token](https://docs.app.wnology.io/applications/application-tokens/) belonging to an application - for instance, getting info on a single token or revoking a token.

* [applicationApiTokens](docs/applicationApiTokens.md)  
  Contains all of the actions that can be performed against the collection of [Api Tokens](https://docs.app.wnology.io/applications/application-tokens/) belonging to an Application - such as listing all tokens or creating a new token.

* [applicationKey](docs/applicationKey.md)  
  Contains all the actions that can be performed against a single [Application Key](https://docs.app.wnology.io/applications/access-keys/) - for instance, getting info on a single key or revoking a key.

* [applicationKeys](docs/applicationKeys.md)  
  Contains all of the actions that can be performed against the collection of [Application Keys](https://docs.app.wnology.io/applications/access-keys/) belonging to an Application - such as listing all keys or creating a new key.

* [auth](docs/auth.md)  
  Contains the actions used for authenticating against the api, either as a user or as a device. The result of authentication calls contain the auth_token needed for authenticated calls - see the examples for more details.

* [dashboard](docs/dashboard.md)  
  Contains all of the actions that can be performed against a single [Dashboard](https://docs.app.wnology.io/dashboards/overview/), which include things like getting info on a dashboard or modifying a dashboard.

* [dashboards](docs/dashboards.md)  
  Contains all of the actions that can be performed against the set of [Dashboards](https://docs.app.wnology.io/dashboards/overview/) that the currently authenticated user has access to - such as listing the dashboards or creating a new dashboard.

* [data](docs/data.md)  
  Contains the actions for querying against historical Device data across an Application.

* [dataTable](docs/dataTable.md)  
  Contains all the actions that can be performed against a single [Data Table](https://docs.app.wnology.io/data-tables/overview/) - for instance, getting info on a single data table or modifying the columns of a data table.

* [dataTables](docs/dataTables.md)  
  Contains all of the actions that can be performed against the collection of [Data Tables](https://docs.app.wnology.io/data-tables/overview/) belonging to an Application - such as listing all data tables or creating a new data table.

* [dataTableRow](docs/dataTableRow.md)  
  Contains all the actions that can be performed against a single row inside of a [Data Table](https://docs.app.wnology.io/data-tables/overview/) - for instance, getting the contents of a row, or modifying a row.

* [dataTableRows](docs/dataTableRows.md)  
  Contains all of the actions that can be performed against the collection of rows that make up a [Data Table](https://docs.app.wnology.io/data-tables/overview/) - such as querying for rows in that table, or adding a new row to the table.

* [device](docs/device.md)  
  Contains all the actions that can be performed against a single [Device](https://docs.app.wnology.io/devices/overview/) - for instance, getting info on a single device or reporting the current state of a device.

* [devices](docs/devices.md)  
  Contains all of the actions that can be performed against the collection of [Devices](https://docs.app.wnology.io/devices/overview/) belonging to an Application - such as listing all devices or sending a command to a set of devices.

* [deviceRecipe](docs/deviceRecipe.md)  
  Contains all the actions that can be performed against a single [Device Recipe](https://docs.app.wnology.io/devices/device-recipes/), which include things like removing a device recipe or creating a device from a device recipe.

* [deviceRecipes](docs/deviceRecipes.md)  
  Contains all the actions that can be performed against the collection of [Device Recipes](https://docs.app.wnology.io/devices/device-recipes/) belonging to an Application - such as listing recipes or creating a new recipe.

* [edgeDeployments](docs/edgeDeployments.md)  
  Contains all the actions that can be performed against the collection of [Edge Deployments](https://docs.app.wnology.io/edge-compute/edge-deployments/) belonging to an Application - such as listing deployments or creating a new deployment.

* [event](docs/event.md)  
  Contains all the actions that can be performed against a single [Event](https://docs.app.wnology.io/events/overview/), such as commenting on or changing the state of an event.

* [events](docs/events.md)  
  Contains all the actions that can be performed against the collection of [Events](https://docs.app.wnology.io/events/overview/) belonging to an Application - such as listing open events or creating a new event.

* [experience](docs/experience.md)  
  Contains all the actions that can be performed against an application [Experience](https://docs.app.wnology.io/experiences/), such as bootstrapping or clearing resources.

* [experienceDomain](docs/experienceDomain.md)  
  Contains all the actions that can be performed against a single [Experience Domain](https://docs.app.wnology.io/experiences/domains/), such as updating SSL certificate information.

* [experienceDomains](docs/experienceDomains.md)  
  Contains all the actions that can be performed against the collection of [Experience Domains](https://docs.app.wnology.io/experiences/domains/) belonging to an Application - such as listing domains or creating a new domain.

* [experienceEndpoint](docs/experienceEndpoint.md)  
  Contains all the actions that can be performed against a single [Experience Endpoint](https://docs.app.wnology.io/experiences/endpoints/), such as updating route information.

* [experienceEndpoints](docs/experienceEndpoints.md)  
  Contains all the actions that can be performed against the collection of [Experience Endpoints](https://docs.app.wnology.io/experiences/endpoints/) belonging to an Application - such as listing endpoints or creating a new endpoint.

* [experienceGroup](docs/experienceGroup.md)  
  Contains all the actions that can be performed against a single [Experience Group](https://docs.app.wnology.io/experiences/groups/), such as updating member information.

* [experienceGroups](docs/experienceGroups.md)  
  Contains all the actions that can be performed against the collection of [Experience Groups](https://docs.app.wnology.io/experiences/groups/) belonging to an Application - such as listing groups or creating a new group.

* [experienceSlug](docs/experienceSlug.md)  
  Contains all the actions that can be performed against a single [Experience Slug](https://docs.app.wnology.io/experiences/domains/), such as changing the associated version.

* [experienceSlugs](docs/experienceSlugs.md)  
  Contains all the actions that can be performed against the collection of [Experience Slugs](https://docs.app.wnology.io/experiences/domains/) belonging to an Application - such as listing slugs or creating a new slug.

* [experienceUser](docs/experienceUser.md)  
  Contains all the actions that can be performed against a single [Experience User](https://docs.app.wnology.io/experiences/users/), such as changing their email or password.

* [experienceUsers](docs/experienceUsers.md)  
  Contains all the actions that can be performed against the collection of [Experience Users](https://docs.app.wnology.io/experiences/users/) belonging to an Application - such as listing users or creating a new user.

* [experienceVersion](docs/experienceVersion.md)  
  Contains all the actions that can be performed against a single [Experience Version](https://docs.app.wnology.io/experiences/), such as modifying the description.

* [experienceVersions](docs/experienceVersions.md)  
  Contains all the actions that can be performed against the collection of [Experience Versions](https://docs.app.wnology.io/experiences/) belonging to an Application - such as listing versions or creating a new versions.

* [experienceView](docs/experienceView.md)  
  Contains all the actions that can be performed against a single [Experience View](https://docs.app.wnology.io/experiences/views/), such as modifying the body template.

* [experienceViews](docs/experienceViews.md)  
  Contains all the actions that can be performed against the collection of [Experience Views](https://docs.app.wnology.io/experiences/views/) belonging to an Application - such as listing views or creating a new view.

* [file](docs/file.md)  
  Contains all the actions that can be performed against a single [File](https://docs.app.wnology.io/applications/files/), such as moving, renaming, or deleting.

* [files](docs/files.md)  
  Contains all the actions that can be performed against the collection of [Files](https://docs.app.wnology.io/applications/files/) belonging to an Application - such as listing files or uploading a new file.

* [flow](docs/flow.md)  
  Contains all the actions that can be performed against a single [Workflow](https://docs.app.wnology.io/workflows/overview/), such as enabling or disabling a workflow, or triggering a virtual button in the workflow.

* [flows](docs/flows.md)  
  Contains all the actions that can be performed against the collection of [Workflows](https://docs.app.wnology.io/workflows/overview/) belonging to an Application - such as listing the workflows or creating a new workflow.

* [flowVersion](docs/flowVersion.md)  
  Contains all the actions that can be performed against a single [Workflow Version](https://docs.app.wnology.io/workflows/versioning/), such as enabling or disabling a workflow version, or updating the version notes.

* [flowVersions](docs/flowVersions.md)  
  Contains all the actions that can be performed against the collection of [Workflow Versions](https://docs.app.wnology.io/workflows/versioning/) belonging to a Workflow - such as listing the versions or creating a new version.

* [me](docs/me.md)  
  Contains the actions for operating against the currently authenticated [User](https://docs.app.wnology.io/user-accounts/overview/) such as changing the password or linking against external services.

* [notebook](docs/notebook.md)  
  Contains all the actions that can be performed against a single [Notebook](https://docs.app.wnology.io/notebooks/overview/), which include things like removing a notebook or executing a notebook.

* [notebooks](docs/notebooks.md)  
  Contains all the actions that can be performed against the collection of [Notebooks](https://docs.app.wnology.io/notebooks/overview/) belonging to an Application - such as listing notebooks or creating a new notebook.

* [integration](docs/integration.md)  
  Contains all the actions that can be performed against a single [Integration](https://docs.app.wnology.io/applications/integrations/), which include things like removing an integration or updating integration configuration.

* [integrations](docs/integrations.md)  
  Contains all the actions that can be performed against the collection of [Integrations](https://docs.app.wnology.io/applications/integrations/) belonging to an Application - such as listing integrations or creating a new integration.

* [org](docs/org.md)  
  Contains all the actions that can be performed against a single [Organization](https://docs.app.wnology.io/organizations/overview/), things like inviting a user to the organization, or modifying the organization.

* [orgs](docs/orgs.md)  
  Contains all of the actions that can be performed against the set of [Organizations](https://docs.app.wnology.io/organizations/overview/) that the currently authenticated user has access to - such as listing the organizations or creating a new organization.

* [webhook](docs/webhook.md)  
  Contains all the actions that can be performed against a single [Webhook](https://docs.app.wnology.io/applications/webhooks/), for instance modifying the verification settings or removing the webhook.

* [webhooks](docs/webhooks.md)  
  Contains all the actions that can be performed against the collection of [Webhooks](https://docs.app.wnology.io/applications/webhooks/) belonging to an Application - such as listing the webhooks or creating a new webhook.

<br/>

*****

Copyright (c) 2022 WEGnology

<https://docs.app.wnology.io/getting-started/what-is-wegnology/>
