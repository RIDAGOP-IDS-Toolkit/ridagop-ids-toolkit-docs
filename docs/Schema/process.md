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
  - **`sequences`** *(object)*
    - **Additional Properties** *(object)*
      - **`title`** *(string)*
      - **`activities`** *(array)*
        - **Items** *(string)*
- **`scriptUri`** *(string/format: uri)*
- **`$comment`** *(string)*
- **`uri`** *(string)*
## Definitions

<h2 id="p-service">P-Service</h2>
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
<h2 id="p-activity">P-Activity</h2>
- **`P-Activity`** *(object)*
  - **`title`** *(string)*
  - **`preProcess`** *(string)*
  - **`bridgeCapability`**: Refer to *[capabilities_names_list](/Schema/capabilities_names_list)*.
  - **`moduleFunction`** *(string)*
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
    - **`resultAsDynamicUI`** *(boolean)*
    - **`resultsAsOpenInput`**
    - **`alert`** *(boolean)*
  - **`debug`** *(object)*
    - **`execute`** *(boolean)*
    - **`resultData`** *(object)*
<h2 id="p-serviceui">P-ServiceUI</h2>
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
<h2 id="p-commonactivity">P-CommonActivity</h2>
- **`P-CommonActivity`** *(object)*
<h2 id="p-postprocess">P-PostProcess</h2>
- **`P-PostProcess`** *(array)*: functions process script file of that process.
  - **Items** *(object)*
    - **`functionName`** *(string)*
    - **`storeKey`** *(string)*
    - **`cacheKey`** *(string)*: key to store a temporary variable, while running this activity.
    - **`followUpProcesses`**: Refer to *[P-PostProcess](#p-postprocess)*.
    - **`parameters`** *(object)*
      - **Additional Properties**: Refer to *[P-Parameter](#p-parameter)*.
<h2 id="p-parameter">P-Parameter</h2>
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
<h2 id="p-servicebridge">P-ServiceBridge</h2>
- **`P-ServiceBridge`** *(object)*
  - **`uri`** *(string/format: uri-reference)*
  - **`instance`**: Refer to *[bridge](/Schema/bridge)*.
<h2 id="p-bridgedefinition">P-BridgeDefinition</h2>
- **`P-BridgeDefinition`** *(object)*
  - **`source`** **(required)** : Refer to *[P-ServiceBridge](#p-servicebridge)*.
  - **`server`**: Refer to *[P-BridgeServerHost](#p-bridgeserverhost)*.
  - **`authorization`** *(object)*
    - **Additional Properties**
<h2 id="p-bridgeserverhost">P-BridgeServerHost</h2>
- **`P-BridgeServerHost`**
<h2 id="p-store">P-Store</h2>
- **`P-Store`** *(object)*
  - **`context`** *(string)*: Must be one of: `['service', 'process', 'activity']`. Default: `service`.
  - **`key`** *(string)* **(required)** : key in the store.
<h2 id="p-storeaccess">P-StoreAccess</h2>
- **`P-StoreAccess`** *(object)*
  - **`context`** *(string)*: Must be one of: `['service', 'process', 'activity']`. Default: `service`.
  - **`key`** *(string)* **(required)** : key in the store.
