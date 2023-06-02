# View modes

Process pages can be displayed in two different view modes `build` and `map`. The mode is set on the
process-page: `view.mode` which is a string either `build` or `map`.If the value is not set, the toolkit will use
the `build` mode.

## Build mode

In build mode, the process page will be completely generated, meaning all input html elements will be generated.

It will create a section for each service in the process.
For each service it will create the following sections:

    - UI Elements section
      All UI elements like inputfields, buttons, ...
    - Status section
      The status of the service, which lists all activities and their sub-activities.
    - Output section
     A Section which contains activities outputs if they contain some.

### UI Elements

These are the available UI elements:

    - Input fields (inputFields)
    - Buttons (buttons)
    - Checkboxes (checkboxes)
    - Select boxes (selects)
    - File inputs (fileInputs)

More detailed information about the UI elements can be found in
the [schema documentation](/schemas/process#p-serviceui).

## Map mode

The ids of the UI elements must follow the given pattern: "input_<service-name>_<inputname>". So for example for the
following intput field `project_id` in the process instance the `id` must be `input_lc_hub_project_id`.

``` json
{
    "instance": {
      "name": "tk_labels",
      "title": "Local Contexts Hub Labels",
      "description": "Add Local Contexts Project reference to a dataset (dataverse)",
      "services": {
        "lc_hub": {
          "title": "LC Hub",
          "ui": {
            "inputFields": {
              "project_id": {
                "label": "LC Hub Project ID",
                "default": ""
              }
            }
           }
       }
   }
}
```
