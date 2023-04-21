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

<figure markdown>
  ![Overview](assets/full-overview.png){ width="700"}
</figure>

Following components of a complete process can be separated into different files json:

- Process-Page
- Process
- Bridge
- Bridge-execution (one of the following)
  - OpenAPI-specification
  - client-module (javascript module)

However, it also possible to combine all components into one file (except the client-module).

In addition, there can be separate javascript modules, that be referenced by specific components:

- Process-Page module
- Process module
- Bridge module

These modules can be used to define custom functions, that can be used for activities which modify some data before or after interacting with a service.
When a function is defined in multiple modules, the one of the most upper one module is used.

Further detail about the components can be found in their respective API documentation.

```json
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "http://localhost:8000/data/schema/idstk_schema/process_page/process_page.schema.json",
  "$comment": "<ProcessPage>",
  "title": "RIDAGOP toolkit ProcessPage-Schema",
  "description": "A process-page description file for the RIDAGOP toolkit",
  "type": "object",
  "properties": {
    "title": {
      "type": "string",
      "description": "The title of the process page"
    },
    "process": {
      "type": "object",
      "title": "Process",
      "description": "The process to be used. Either the full instance or a uri to the process description.",
      "$comment": "formatted like this, there are no strict-mode errors",
      "properties": {
        "$comment": {
          "type": "string"
        },
        "uri": {
          "type": "string",
          "title": "URI to process",
          "description": "The uri to the process description.",
          "format": "uri"
        },
        "instance": {
          "title": "Process instance",
          "description": "The full process instance, which needs to be a valid Process",
          "$ref": "/data/schema/idstk_schema/process/process.schema.json"
        }
      },
      "oneOf": [
        {
          "properties": {
            "uri": {
              "title": "URI to process",
              "description": "The uri to the process description.",
              "type": "string",
              "format": "uri"
            }
          },
          "required": [
            "uri"
          ]
        },
        {
          "properties": {
            "instance": {
              "title": "Process instance",
              "description": "The full process instance, which needs to be a valid Process",
              "$ref": "/data/schema/idstk_schema/process/process.schema.json"
            }
          },
          "required": [
            "instance"
          ]
        }
      ],
      "additionalProperties": false
    },
    "services": {
      "type": "object",
      "title": "Map of services (service-name: description)",
      "description": "Process-page description for the services to be used in the process. The services here must match the services in the process.",
      "additionalProperties": {
        "$ref": "#/$defs/PP-Service"
      },
      "minProperties": 1
    },
    "uri": {
      "$comment": "not sure why this is here",
      "type": "string"
    },
    "view": {
      "type": "object",
      "title": "Page view mode",
      "description": "type property defines if the page view should be generated (build) or if specified ui elements should be mapped to existing ui elements in the html page",
      "properties": {
        "type": {
          "type": "string",
          "enum": [
            "build",
            "map"
          ]
        }
      }
    },
    "scriptUri": {
      "title": "ProcessPage schema uri",
      "description": "url to the process-page module. this module will be merged with the process module. Activities can use the module function when they specify a 'moduleFunction' instead of 'bridgeCapability'.",
      "type": "string",
      "format": "uri-reference"
    },
  "required": [
    "process",
    "services",
    "schemaUri"
  ],
```

### RIDAGOP toolkit ProcessPage-Schema

_A process-page description file for the RIDAGOP toolkit_

__type__: object

__properties__:

  - title: _(string)_ The title of the process page
  - process: _(object)_ __(required)__ The process to be used. Either the full instance or a uri to the process description.
  - services: __(required)__ Process-page description for the services to be used in the process. The services here must match the services in the process.
  - view: The type property of the view defines if the page view should be generated (build) or if specified ui elements should be mapped to existing ui elements in the html page
  - schemaUri: _(string/format:uri-reference)_ __(required)__ Absolute or relative url to the process-page module. this module will be merged with the process module. Activities can use the module function when they specify a 'moduleFunction' instead of 'bridgeCapability'.
  
