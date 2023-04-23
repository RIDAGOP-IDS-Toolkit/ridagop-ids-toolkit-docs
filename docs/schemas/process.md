# RIDAGOP toolkit Process-Schema

*A process in the IDS toolkit*

## Properties

- **`name`** *(string)* **(required)** : The name of the process. This is the name for internal identification (although there is only one process).
- **`title`** *(string)*: The title of the process.
- **`description`** *(string)*: A description of the process. (For other developers).
- **`services`** *(object)* **(required)** : Services with their UI-elements and activities.
    - **Additional Properties**: Each key specifies the name of a service. Refer to *[P-Service](#p-service)*.
- **`common`** *(object)*: Common activities and ui elements (access to activities of other services). Default: `{'ui': {}, 'activities': {}}`.
    - **`ui`** *(object)*: Default: `{}`.
        - **`buttons`** *(object)*
            - **Additional Properties** *(object)*
                - **`label`** *(string)* **(required)** 
                - **`triggerActivity`** *(string)* **(required)** 
                - **`activityService`** *(string)*
                - **`triggerSequence`** *(string)*
    - **`activities`** *(object)*
        - **Additional Properties** *(object)*
            - **`One of (1)`**: Refer to *[P-Activity](#p-activity)*.
            - **`One of (2)`**: Refer to *[P-CommonActivity](#p-commonactivity)*.
    - **`sequences`** *(object)*
        - **Additional Properties** *(object)*
            - **`title`** *(string)*
            - **`activities`** *(array)*
                - **Items** *(string)*
- **`scriptUri`** *(string/format: uri-reference)*: The URI of the script that is used to run the process.
## Definitions

## P-Service
- **`P-Service`** *(object)*: A service as it is defined in the Process. Includes UI-elements, activities and shared parameters and the bridge.
    - **`title`** *(string)*: The user visible title of the service.
    - **`ui`**: The UI-elements of the service. Refer to *[P-ServiceUI](#p-serviceui)*. Default: `{}`.
    - **`parameters`** *(object)*: Parameters that are usable by all activities of this service.
        - **Additional Properties**: Refer to *[P-Parameter](#p-parameter)*.
    - **`autostart`**: Activities that are started on page load.
    - **`bridge`**: The bridge of this service. Refer to *[P-BridgeDefinition](#p-bridgedefinition)*.
    - **`activities`** *(object)*: The activities of this service.
        - **Additional Properties**: Each key specifies the name of an activity. Refer to *[P-Activity](#p-activity)*.
    - **`sequences`** *(object)*: A sequence is a list of activities. This is for convenience for tirggereing multiple activities at once.
        - **Additional Properties** *(object)*: Each key specifies the name of a sequence.
            - **`title`** *(string)*: The user visible title of the sequence.
            - **`activities`** *(array)*: The list of activities that are executed when the sequence is triggered.
                - **Items** *(string)*: The name of an activity.
## P-Activity
- **`P-Activity`** *(object)*: An activity which uses the bridge or a module function.
    - **`preProcess`** *(string)*: a module function that is called before the activity is executed.
    - **`priority`** *(integer)*
    - **`comment`** *(string)*
    - **`parameters`** *(object)*: Parameters that are usable by this activity.
        - **Additional Properties**: A parameter-name defined by the bridgeCapability or moduleFunction. Refer to *[P-Parameter](#p-parameter)*.
    - **`requestBody`** *(object)*: The request body of the activity (only relevant when using OpenAPI execution).
        - **Additional Properties** *(object)*: Used when the body is multipart/form-data.
        - **`data`** *(object)*: Used when the body is a JSON object.
            - **`fileInput`** *(string)*
            - **`store`**: Refer to *[P-StoreAccess](#p-storeaccess)*.
    - **`requiredActivities`** *(array)*
        - **Items** *(object)*
            - **`serviceName`** *(string)*
            - **`activityName`** *(string)* **(required)** 
            - **`errorMessage`** *(string)*
    - **`storeResult`**: Storing the result of the activity for later use. Refer to *[P-Store](#p-store)*.
    - **`subActivities`** *(object)*: Sub-activities are activities that are executed after this parent activity is executed.
        - **Additional Properties**: Each key specifies the name of a sub-activity. Refer to *[P-Activity](#p-activity)*.
    - **`ui`** *(object)*: Some UI related settings for the activity.
        - **`includeInStatus`** *(boolean)*
        - **`resultAsOutputHtml`**
            - **`One of (1)`** *(boolean)*
            - **`One of (2)`** *(string)*
        - **`resultAsDynamicUI`** *(boolean)*
        - **`resultsAsOpenInput`**
            - **`One of (1)`** *(boolean)*
            - **`One of (2)`** *(string)*: Must be one of: `['start', 'end']`.
        - **`alert`** *(boolean)*
    - **`debug`** *(object)*: Some debug related settings for the activity.
        - **`execute`** *(boolean)*
        - **`resultData`** *(object)*
    - **`One of (1)`**
        - **`title`** *(string)* **(required)** : The user visible title of the activity.
        - **`bridgeCapability`** **(required)** : The name of the bridge capability that is used to execute the activity. Refer to *[capabilities_names_list](/Schema/capabilities_names_list)*.
    - **`One of (2)`**
        - **`title`** *(string)* **(required)** : The user visible title of the activity.
        - **`moduleFunction`** *(string)* **(required)** : The name of the module function (??? FROM WHERE) that is used to execute the activity.
## P-ServiceUI
- **`P-ServiceUI`** *(object)*: The UI of a service. Consider that many of the properties are only relevant when the Ui is generated. Default: `{}`.
    - **`inputFields`** *(object)*
        - **Additional Properties** *(object)*: key is the name of the input field.
            - **`label`** *(string)*: The label of the input field.
            - **`fromQueryParam`** *(string)*: If this property is set, the field is initiated with the value of the query parameter.
            - **`default`** *(string)*: The default value of the input field.
            - **`textArea`** *(boolean)*: If true, the input field is a text area (which is nicer for longer inputs.
            - **`inputActions`** *(object)*: Adds a small button next to the input. Clicking that or pressing enter triggers an action.
                - **`autoAction`** *(string)*: The name of the action that is triggered.
    - **`buttons`** *(object)*
        - **Additional Properties** *(object)*
            - **`label`** *(string)*
            - **`triggerActivity`** *(string)*
            - **`triggerSequence`** *(string)*
    - **`checkBoxes`** *(object)*
        - **Additional Properties** *(object)*
            - **`label`** *(string)*
            - **`default`** *(boolean)*: Default: `False`.
    - **`selects`** *(object)*
        - **Additional Properties** *(object)*
            - **`label`** *(string)*
            - **`options`** *(array)*
                - **Items** *(object)*
                    - **`label`** *(string)* **(required)** 
                    - **`value`** *(string)* **(required)** 
            - **`default`** *(string)*
    - **`fileInputs`** *(object)*
        - **Additional Properties** *(object)*
            - **`label`** *(string)*
            - **`accept`** *(string)*
            - **`binary`** *(boolean)*
            - **`readImmediately`** *(boolean)*
            - **`keepAsFile`** *(boolean)*
## P-CommonActivity
- **`P-CommonActivity`** *(object)*
## P-PostProcess
- **`P-PostProcess`** *(array)*: functions process script file of that process.
    - **Items** *(object)*
        - **`functionName`** *(string)*
        - **`storeKey`** *(string)*
        - **`cacheKey`** *(string)*: key to store a temporary variable, while running this activity.
        - **`followUpProcesses`**: Refer to *[P-PostProcess](#p-postprocess)*.
        - **`parameters`** *(object)*
            - **Additional Properties**: Refer to *[P-Parameter](#p-parameter)*.
## P-Parameter
- **`P-Parameter`** *(object)*
    - **`type`** *(string)*: Must be one of: `['string', 'number', 'boolean']`.
    - **`parent`**
    - **`previous`**
    - **`field`** *(string)*
    - **`queryParam`** *(string)*
    - **`constant`** *(string)*
    - **`fileInput`** *(string)*
    - **`store`**: Refer to *[P-StoreAccess](#p-storeaccess)*.
    - **`generate`** *(object)*
        - **`bridgeCapability`** *(string)*
        - **`moduleFunction`** *(string)*
        - **`parameters`** *(object)*
            - **Additional Properties** *(object)*
                - **Additional Properties**: Refer to *[P-Parameter](#p-parameter)*.
    - **`comment`** *(string)*
    - **`dynamic`** *(boolean)*
## P-ServiceBridge
- **`P-ServiceBridge`** *(object)*
    - **`uri`** *(string/format: uri-reference)*
    - **`instance`**: Refer to *[bridge](/Schema/bridge)*.
## P-BridgeDefinition
- **`P-BridgeDefinition`** *(object)*
    - **`source`** **(required)** : Refer to *[P-ServiceBridge](#p-servicebridge)*.
    - **`server`**: Refer to *[P-BridgeServerHost](#p-bridgeserverhost)*.
    - **`authorization`** *(object)*
        - **Additional Properties**
## P-BridgeServerHost
- **`P-BridgeServerHost`**
    - **`One of (1)`** *(string/format: uri)*
    - **`One of (2)`** *(object)*
        - **`field`** *(string)* **(required)** 
    - **`One of (3)`** *(object)*
        - **`queryParam`** *(string)* **(required)** 
    - **`One of (4)`** *(object)*
        - **`constant`** *(string)* **(required)** 
## P-Store
- **`P-Store`** *(object)*
    - **`context`** *(string)*: Must be one of: `['service', 'process', 'activity']`. Default: `service`.
    - **`key`** *(string)* **(required)** : key in the store.
## P-StoreAccess
- **`P-StoreAccess`** *(object)*
    - **`context`** *(string)*: Must be one of: `['service', 'process', 'activity']`. Default: `service`.
    - **`key`** *(string)* **(required)** : key in the store.
