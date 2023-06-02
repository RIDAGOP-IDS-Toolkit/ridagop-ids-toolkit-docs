# Technical overview

The IDS toolkit is a simple javascript module that can be integrated into any website.
When the module is loaded on a webpage, a process-page can be initiated by calling the global `_ids_init_` function,
with the url of the process-page description file as argument.

The Process-Page is a json file that describes the process-page. It links (or includes) the process, which describe the
services of a process, which contain activities.

The essential parts that define what a process can do, and how a user can interact with it, are the activities and the
ui-elements of services.

The toolkit provides a set of json-schema files, that can be used to validate the process-page, process, bridge. There
is also a json-schema file that integrates all of them and provides validation for an integrated proces-page description
file.
The json-schemas are described here in more detail in [the Schemas section](/schemas).

## Installation and setup

The javascript can be integrated like this:

``` html
<!-- index.html -->
...
<script src="path/to/toolkit/index.js"></script>
...
<body>
  <!-- your static content in case the process-page is not generating the page -->
  <script>
      window.onload = function() {
          try {
              _init_toolkit_(`path/to/process-file.json`)
          } catch (e) {
              console.error(e)
          }
      }
  </script>
</body>
```

While developing new processes it is recommended to start generating the page and then switch to map-mode, when all
activities and interactions with the services are set.

While writing the instances of the individual components it might be useful to use a json-schema validator, to constantly check the validity of the json files.
There are 4 json-schema files. One for the process-page, one for the process and one for the bridge and one that combines all of them, which is a extended json-schema for a process-page (which can integrate the other instances).
The json-schema files can be found here in the [toolkit repository](https://github.com/RIDAGOP-Toolkit/ridagop-toolkit/tree/main/schemas).
A documentation for the individual components [can be found here](/schemas).

The toolkit has a robust validation system integrated, next to the json-schema validation raising errors about missing
or wrong defined components.

## Overview


<figure markdown>
  ![Overview](/assets/full-overview.png){ width="700" }
</figure>

Following components of a complete process can be separated into different json files:

- Process-Page
- Process
- Bridge
- Bridge-execution (one of the following)
    - OpenAPI-specification
    - client-module (javascript module)

However, it also possible to combine all components into one file (except the OpenAPI spec and the client-module).

In addition, there can be separate javascript modules, that be referenced by specific components:

- Process-Page module
- Process module
- Bridge module

These modules can be used to define custom functions, that can be used for activities which modify some data before or
after interacting with a service.
When a function is defined in multiple modules, the one of the most upper one module is used.

Further detail about the components can be found in their respective API documentation.

### View modes

Process pages can be displayed in two different view modes.
Either the page can be completely generated, meaning all input html elements will be generated (build-mode)
in map-mode, where the defined interaction elements will be mapped to existing html on the page.
Read more about the 2 view modes on the [User interface page](/ui).

## Process

A process defines a set of __services__, which describe how the user can interact with external services.

A service, which has a bridge defines a set of activities and ui-elements that can be used to interact with the service.
Next to the services the process can also define a `common` object, activities and ui-elements.
This is particular useful, when activities, need to call other activities from other services (in subActivities).


Read here full details of the [process schema](schemas/process).

### Services

Services are defined within a process and represent the interactions with external services that is used in the process.
However the main part of a services, 
Most importantly the process-page definition of a service can ([full schema](/schemas/process-page#pp-service)):

- define a new bridge
- define new autostart activities (<- ??? REF)
- specify to show/hide sections of the UI
- Define additional service-wide parameters
- define additional text-fields and buttons (that can trigger activities)

The process instance defines the crucial parts of a service, which are ([full schema](/schemas/process#p-service)):

- The bridge
- activities
- autostart activities
- sequences
- service wide parameters

#### UI-Elements

The UI-Elements are defined per service. There are five types of UI-Elements that can be defined, which can be used for
activities:

- Input fields: For text input
- Buttons: Buttons can be used to trigger activities
- Selects: For selecting a value from a list
- Checkboxes: To select from a boolean option
- File-inputs: To load files that can be used as input for activities

See the [UI-Elements section](/ui.md) for more details.

## Activities

Activities are defined in the process and describe the interaction with a service. Activities are referring to some
execution, which is defined in the bridge of the service.

### Activity execution

Every activity needs to specify what execution of the bridge is should invoke. This is done by specifying
the `bridgeCapability` or the `moduleFunction` property.
In the case of the `bridgeCapability` property, it will invoke a capability that is defined by the bridge of the service.
In the case of the `moduleFunction` property, it will invoke a function that is defined in the module that the process-page or process include (with `scriptUri`). 

TODO!!!! Bridge supportModule?? for OpenAPI bridges?!?!


### Required activities

Activities can refer to other activities (of the same service) that need to be executed before, this activity can be
executed. This is in order to guarantee that some data, that is required by an activity are loaded/processed (by the
required activities).

### Activity parameter

Activities generally require some parameters, which are the parameters of the execution of the activity. The parameter
description defines where the values of the parameters are coming from. The following parameter sources are possible:

- Parent
- Previous
- Field
- QueryParam
- Constant
- FileInput
- Store
- Generate
- Dynamic

### Preprocess execution

It is also possible to refer to a module-function that is executed before the activity is executed.
The preprocess activity takes the same parameters as the actual activity in form of a json-object (keys are parameter
names) and can return json objects, where every key overwrites the parameter passed to the actual activity.
Preprocess can also throw Errors, with the `cause`: `cancel`, in order to cancel the activity execution.

### Handling activity results

#### Storing results

The results of an activity can be stored in order to use them in other activities. The result can be stored in three
different contexts, which defines, which other activities can access them. These contexts are:

- Process: The result can be accessed by all activities of the process
- Service: The result can be accessed by all activities of the service
- Activity: The result can be accessed by sub-activities

#### Special results handling

Besides storing the results, there are also some special results handling options:

##### Html Output

The result of of an activity can be some HTML, which can be displayed in the UI.

For generated UIs there is a standard output section for each service, in that case, the property `resultAsOutputHtml`
just needs to be `true`.
For mapped UIs, the property `resultAsOutputHtml` should specify a `string`, which should be the `id` of the html
element, where the result should be displayed.

##### Dynamic UI Elements

The result of an activity can also be used to dynamically generate UI elements. The result of the activity should be a
json-object that has the same structure of service uis (schema: [Process Service-UI](/schemas/process#p-serviceui))
The Ui-elements are added the Html-Element with the id `{service.name}_dyn_ui`.

### Sub-activities

An activity can also define sub-activities. These sub-activities are executed after the activity main (`parentActivity`)
is executed.

### Bridge

Each service requires a bridge, which defines how the activities are executed. There are two basic types of bridge executions:

- OpenAPI: The bridge is defined by an OpenAPI specification. The activities are executed by calling the endpoints of
  the OpenAPI specification. Learn more about OpenAPI at [openapi.org](https://www.openapis.org/)
- Client-Module: The bridge is defined by a javascript module, which defines functions that can be called by the
  activities. 


