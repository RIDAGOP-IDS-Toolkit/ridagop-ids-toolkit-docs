# Local Contexts Hub Label

## Introduction

...

## Structure

The Local Contexts Hub Labels Process is available as a generated gui version and with a html page, which includes
a specific UI for that process.

The specific instance files that are used for the process are:

```
  process_page/lc_hub_labels.json
  bridge/dataverse_bridge_OPENAPI.json
  openapi/dataverse3.json
  openapi/localcontextshub.json
```

The process uses 2 services, Local Contexts Hub and Dataverse, which are both connected with OpenAPI bridge.
The Bridge-definition of dataverse is in a specific json file, while the one for Local Contexts Hub is in the
process-page file.

In addition to the bridge, the process also includes a process module,  `scripts/process/lc_hub_labels.js`.

The User inputs for the 3 services contain the following input fields:

for the lc_hub service...

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

and

for the data_repo (dataverse) service...

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

The Local Contexts service has only the one following activity:

- Fetch project data, which fetches the LC Hub project metadata of the project given in the `project_id` input field
    - Sub-activity: Display project labels, which displays the Labels of the project in the service output section

The Dataverse service has 2 activities:

- Fetch dataset metadata"
    - Sub-activity: find LCHub Project Reference
        - Sub-activity: Create LCHub Reference text
        - Sub-activity: Update the metadata of the dataset
        - Sub-activity: Display updated dataset description
