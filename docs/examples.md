# Process examples

## Local Contexts Hub Labels - Dataverse

### Introduction

[Local Contexts](https://localcontexts.org) is a global initiative that supports Indigenous communities with tools that
attribute cultural authority of heritage and data.
By focusing on Indigenous Cultural and Intellectual Property and Indigenous Data Sovereignty, Local Contexts helps
Indigenous communities repatriate knowledge and gain control over how their data is collected, managed, displayed,
accessed, and used in the future.

The Local Contexts Project developed TK and BC Labels (traditional knowledge and Biocultural) through sustained
partnership within Indigenous communities. “The Labels allow communities to express local and specific conditions for
sharing and engaging in future research and relationships in ways that are consistent with already existing community
rules,
governance and protocols for using, sharing and circulating knowledge and data.”

The [Local Contexts Hub](https://localcontextshub.org/) works in tandem with already existing information/collections
management systems and tools. The Hub generates Labels and Notices (with permanent identifiers) so these can be added to
already existing catalog and collections management systems.

### Structure of the Process

The Local Contexts Hub Labels Process is available as a generated user interface version and with a html page, which
includes
UI elements for that process.

The specific instance files that are used for the process are:

```
  process_page/lc_hub_labels.json
  bridge/dataverse_bridge_openapi.json
  openapi/dataverse_openapi.json
  openapi/localcontextshub_openapi.json
```

The process uses 2 services, Local Contexts Hub and Dataverse, which are both connected with OpenAPI bridge.
The Bridge-definition of Dataverse is in a specific json file, while the one for Local Contexts Hub is in the
process-page file. This is because we already use the Dataverse bridge in other processes, while the Local Contexts Hub
is only used in the process so far, so we can simply integrate it into the process page file.

In addition to the bridge, the process also includes a process module,  `scripts/process/lc_hub_labels.js`.

### User interface elements

The User inputs for the services contain the following input fields:

for the lc_hub service, we only need one input field, the project id, from which we want to use the Labels
and a button, which will pull the Labels from the Hub, in order to push them to the Dataverse dataset.

```json
{
  "inputFields": {
    "project_id": {
      "label": "LC Hub Project ID"
    }
  },
  "buttons": {
    "fetch_labels": {
      "label": "Fetch labels",
      "triggerActivity": "read_lc_hub_data"
    }
  }
}
``` 

and for the Dataverse service (which is just called data_repo) we have 3 input fields:

- the Dataverse instance
  Since Dataverse is not one centralized repository, we need to specify the instance, which is identified by its Web
  address (URL).
- The dataset identifier
  The dataset identifier is the DOI of the dataset, which we want to update with the Labels.
- The API Key
  The API Key is used to authorize the user, who wants to update the dataset metadata, which needs to
  be [obtained from Dataverse](https://guides.dataverse.org/en/5.13/user/account.html#how-to-create-your-api-token).

For demonstration purposes, we have set the Dataverse instance to a default value and added an input action to the input
field for the dataset identifier, which will automatically read the dataset metadata from the Dataverse instance, when
the user enters the dataset identifier.
This will add a small button to the input field, which will trigger the activity `read_dataset` when clicked, but also
when the user presses enter after entering the dataset identifier. Lastly, there is a button, which
after getting information, if the Labels are already exising and up to-date in the dataset metadata, will update the
dataset metadata with the Labels.

```json
{
  "inputFields": {
    "dataverseInstance": {
      "label": "Dataverse instance",
      "default": "https://ridagop.net"
    },
    "data_repo_id": {
      "label": "Dataverse dataset doi",
      "inputActions": {
        "autoAction": "read_dataset"
      }
    },
    "apiKey": {
      "label": "API Key"
    }
  },
  "buttons": {
    "udpate": {
      "label": "Update Metadata",
      "triggerActivity": "postDatasetMetadata"
    }
  }
}
```

### Activities

Now we will see the activities defined in the two services. After a general overview and the complete json of the
activities definition, we look at individual feature of the toolkit that allow to describe how activities work.

The Local Contexts service has one activity:

- Fetch project data, which fetches the LC Hub project metadata of the project given in the `project_id` input field
    - Sub-activity: Display project labels, which displays the Labels of the project in the service output section

``` json
{
    "read_lc_hub_data": {
      "title": "Fetch project data",
      "bridgeCapability": "read_dataset_metadata",
      "preProcess": "checkProjectID",
      "parameters": {
        "project_id": {
          "field": "project_id"
        }
      },
      "storeResult": {
        "context": "process",
        "key": "lc_hub_project_labels"
      },
      "subActivities": {
        "displayLabels": {
          "title": "Display project labels",
          "moduleFunction": "display_project_labels",
          "parameters": {
            "lc_hub_project_data": {
              "parent": true
            }
          },
          "ui": {
            "resultAsOutputHtml": true
          }
        }
      },
      "comment": "input_field can be omitted if there is an input_actions set"
    }
}
```

The Dataverse service has 2 activities:

- Fetch dataset metadata, which fetches the dataset metadata from the Dataverse instance, using the dataset identifier.
  The following sub-activities are executed and apply changes to the dataset metadata within the browser:
    - Sub-activity: find LCHub Project Reference, which checks if the dataset metadata already contains a reference to a
      local contexts project
        - Sub-activity: Create LCHub Reference, which transforms the local contexts project metadata into the format
          needed for the dataverse metadatablock
        - Sub-activity: Update the metadata of the dataset, which inserts the Local Contexts Hub reference into the
          dataset metadata
        - Sub-activity: Display updated dataset description, which displays the updated dataset metadata in the service
          output section

- Post updated metadata, which updates the dataset on Dataverse (pushing the changes to the Dataverse instance)
    - Publish updated dataset, which publishes the dataset on Dataverse (Since making updates to the dataset, will
      change the status to draft)

```json
{
  "read_dataset": {
    "title": "Fetch dataset metadata",
    "storeResult": {
      "key": "dataset"
    },
    "requiredActivities": [
      {
        "serviceName": "lc_hub",
        "activityName": "read_lc_hub_data",
        "errorMessage": "Please fetch the Local Contexts project Labels first"
      }
    ],
    "parameters": {
      "persistentId": {
        "field": "data_repo_id"
      }
    },
    "bridgeCapability": "read_dataset_metadata",
    "subActivities": {
      "findReference": {
        "moduleFunction": "findLCHubProjectReference",
        "title": "Find LCHub Project Reference",
        "parameters": {
          "datasetData": {
            "parent": true
          },
          "lc_hub_project_data": {
            "store": {
              "context": "process",
              "key": "lc_hub_project_labels"
            }
          }
        },
        "storeResult": {
          "key": "referenceFound"
        },
        "subActivities": {
          "createLCHubReference": {
            "title": "Create LCHub Reference",
            "moduleFunction": "createLCHubReference",
            "priority": 1,
            "parameters": {
              "lc_hub_project_data": {
                "store": {
                  "context": "process",
                  "key": "lc_hub_project_labels"
                }
              }
            },
            "storeResult": {
              "key": "referenceData"
            }
          },
          "updateDatasetMetadata": {
            "title": "Update the metadata of the dataset",
            "moduleFunction": "updateDatasetMetadata",
            "storeResult": {
              "key": "edited_metadata"
            },
            "parameters": {
              "datasetData": {
                "store": {
                  "context": "service",
                  "key": "dataset"
                }
              },
              "referenceFound": {
                "store": {
                  "context": "service",
                  "key": "referenceFound"
                }
              },
              "referenceData": {
                "store": {
                  "context": "service",
                  "key": "referenceData"
                }
              }
            }
          },
          "display_updated_description": {
            "title": "Display updated dataset description",
            "moduleFunction": "display_updated_description",
            "parameters": {
              "referenceFound": {
                "store": {
                  "context": "service",
                  "key": "referenceFound"
                }
              }
            },
            "ui": {
              "resultAsOutputHtml": "udpated_dataset"
            }
          }
        }
      }
    }
  },
  "postDatasetMetadata": {
    "title": "Post updated metadata",
    "bridgeCapability": "update_dataset_metadata",
    "parameters": {
      "persistentId": {
        "field": "data_repo_id"
      },
      "versionId": {
        "constant": ":draft"
      }
    },
    "requestBody": {
      "data": {
        "store": {
          "context": "service",
          "key": "edited_metadata"
        }
      }
    },
    "subActivities": {
      "publishUpdatedDataset": {
        "title": "Publish updated dataset",
        "bridgeCapability": "publish_dataset",
        "preProcess": "shouldPublish",
        "parameters": {
          "persistentId": {
            "field": "data_repo_id"
          },
          "type": {
            "constant": "minor"
          }
        }
      }
    }
  }
}
```

#### Activities properties:

- bridgeCapability, moduleFunction:
As explained before, each activity needs either a `bridgeCapability` or a `moduleFunction` property, specifying what bridge capability or process function should be invoked.

For the Local Contexts service we use the bridge capability: read_dataset_metadata and the module function: display_project_labels.

For the Dataverse service we use the bridge capabilities: read_dataset_metadata, update_dataset_metadata and publish_dataset and the module functions: findLCHubProjectReference, createLCHubReference, updateDatasetMetadata and display_updated_description.

- proProecess: A module function that should be called before calling the actual execution. This can modify the parameters or cancel the activity execution.

For the Dataverse sub-activity _publishUpdatedDataset_ of the activity _postDatasetMetadata_ uses the preProcess function shouldPublish, which will check if the dataset, was a draft before modifying it. If it was a draft, it will not publish the dataset.

- parameters

- storeResult

- subActivities

- ui/resultAsOutputHtml

- requiredActivities

- priority

- requestBody

## Data Access Request
