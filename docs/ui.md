# User interface / View modes

Process pages can be displayed in two different view modes `build` and `map`. The mode is set on the
process-page: `view.mode` which is a string either `build` or `map`.If the value is not set, the toolkit will use
the `build` mode.

## Build mode

In build mode, the process page will be completely generated, meaning all input html elements will be generated.

It will create a section for each service in the process.
For each service it will create the following sections `services.<service-name>.ui` and `common.ui`:

    - UI Elements section (`input` object)
      All UI elements like inputfields, buttons, ...
    - Status section  (`status` object)
      The status of the service, which lists all activities and their sub-activities.
    - Output section (`output` object)
     A Section which contains activities outputs if they contain some.

All 3 sections can be hidden with the `display` set to `false` in the respective objects.
Also, a whole service section can be hidden. In addition the `input` section can have additional attributes
to create UI blocks, which remove the ui elements from their default position and structure them and to change the
default value of input fields and hide them.

Each service section will also contain 2 containers for dynamically added ui-elements, one (default) after the normal UI
section and another before.

More detailed information about the UI elements can be found in
the [schema documentation](/schemas/process-page#pp-uisettings).

### UI Elements

These are the available UI elements for the services and the common section:

- Input fields: `inputFields` For text input
- Buttons: `buttons`  can be used to trigger activities and sequences (given by their name). _Buttons in the common section
  can trigger activities of all other services._
- Selects: `selects` For selecting a value from a list
- Checkboxes: `checkBoxes` To select from a boolean option
- File-inputs: `fileInputs` To load files that can be used as input for activities

They are defined in the process instance in `services.<service-name>.ui`

[More info on all properties: schema documentation](/schemas/process#p-serviceui).

## Map mode

The ids of the UI elements must follow the given pattern: `<input-type>_<service-name>_<inputname>`. So for example for
the following __intput field__ `project_id` in the local contexts hub
service (named `lc_hub` in the process) the `id` must be `input_lc_hub_project_id`.

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

The input types are following:

- Buttons: `button`
- Checkboxes: `checkbox`
- Select boxes: `select`
- File inputs: `fileinput`
