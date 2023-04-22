# RIDAGOP toolkit Process-Schema

*A process in the IDS toolkit*

## Properties

- **`name`** *(string)* **(required)** 
- **`title`** *(string)*
- **`description`** *(string)*
- **`services`** *(object)* **(required)** : Services with their input_fields and activities.
    - **Additional Properties**: Refer to *[P-Service](#p-service)*.
- **`common`** *(object)*: Common activities and ui elements. Default: `{'ui': {}, 'activities': {}}`.
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
- **`scriptUri`** *(string/format: uri)*
- **`$comment`** *(string)*
- **`uri`** *(string)*
## Definitions

## P-Service
- **`P-Service`** *(object)*
    - **`title`** *(string)*
    - **`ui`**: Refer to *[P-ServiceUI](#p-serviceui)*. Default: `{}`.
    - **`parameters`** *(object)*
        - **Additional Properties**: Refer to *[P-Parameter](#p-parameter)*.
    - **`autostart`**
    - **`bridge`**: Refer to *[P-BridgeDefinition](#p-bridgedefinition)*.
    - **`activities`** *(object)*
        - **Additional Properties**: Refer to *[P-Activity](#p-activity)*.
    - **`sequences`** *(object)*
        - **Additional Properties** *(object)*
            - **`title`** *(string)*
            - **`activities`** *(array)*
                - **Items** *(string)*
## P-Activity
- **`P-Activity`** *(object)*
    - **`preProcess`** *(string)*
    - **`priority`** *(integer)*
    - **`comment`** *(string)*
    - **`parameters`** *(object)*
        - **Additional Properties**: Refer to *[P-Parameter](#p-parameter)*.
    - **`requestBody`** *(object)*
        - **Additional Properties** *(object)*
        - **`data`** *(object)*
            - **`fileInput`** *(string)*
            - **`store`**: Refer to *[P-StoreAccess](#p-storeaccess)*.
    - **`requiredActivities`** *(array)*
        - **Items** *(object)*
            - **`serviceName`** *(string)*
            - **`activityName`** *(string)* **(required)** 
            - **`errorMessage`** *(string)*
    - **`storeResult`**: Refer to *[P-Store](#p-store)*.
    - **`subActivities`** *(object)*
        - **Additional Properties**: Refer to *[P-Activity](#p-activity)*.
    - **`ui`** *(object)*
        - **`includeInStatus`** *(boolean)*
        - **`resultAsOutputHtml`**
            - **`One of (1)`** *(boolean)*
            - **`One of (2)`** *(string)*
        - **`resultAsDynamicUI`** *(boolean)*
        - **`resultsAsOpenInput`**
            - **`One of (1)`** *(boolean)*
            - **`One of (2)`** *(string)*: Must be one of: `['start', 'end']`.
        - **`alert`** *(boolean)*
    - **`debug`** *(object)*
        - **`execute`** *(boolean)*
        - **`resultData`** *(object)*
    - **`One of (1)`**
        - **`title`** *(string)* **(required)** 
        - **`bridgeCapability`** **(required)** : Refer to *[capabilities_names_list](/Schema/capabilities_names_list)*.
    - **`One of (2)`**
        - **`title`** *(string)* **(required)** 
        - **`moduleFunction`** *(string)* **(required)** 
## P-ServiceUI
- **`P-ServiceUI`** *(object)*: Default: `{}`.
    - **`inputFields`** *(object)*
        - **Additional Properties** *(object)*
            - **`label`** *(string)*
            - **`fromQueryParam`** *(string)*
            - **`readonly`** *(boolean)*
            - **`default`** *(string)*
            - **`textArea`** *(boolean)*
            - **`inputActions`** *(object)*
                - **`autoAction`** *(string)*
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
