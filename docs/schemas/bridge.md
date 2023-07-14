# RIDAGOP toolkit Bridge-Schema

*Describes the capabilities.*

## Properties

- **`execute`** **(required)** : Defines the type of execution. Either by using an OpenAPI-Specification or by using a client library.
    - **`One of (1)`** *(object)*: OpenAPI-Specification.
        - **`openapiSchemaUri`** *(string/format: uri-reference)* **(required)** : The URI of the OpenAPI-Specification.
    - **`One of (2)`** *(object)*: Client library module.
        - **`apiClientModuleUri`** *(string/format: uri-reference)* **(required)** : The URI of the module that contains the client library.
- **`capabilities`** *(object)* **(required)** : Defines the capabilities of the bridge.
    - **`Any of (1)`**
        - **Additional Properties**: Refer to *[Capability](#capability)*.
    - **`Any of (2)`**
        - **`^_.*$`**: Refer to *[Capability](#capability)*.
- **`errorMessagePath`** *(string)*: Relative json-path in the response, where an error-message is located, if the response is not a 2XX answer.
- **`supportModuleUri`** *(string/format: uri-reference)*: The URI of the module that contains the support functions.
## Definitions

## Capability
- **`Capability`** *(object)*: Each name defines a capability. The allowed names are defined in the capabilities_names_list.json file.
    - **`Any of (1)`**
        - **`operation`** **(required)** : Refer to *[B-Operation](#b-operation)*.
    - **`Any of (2)`**
        - **`functionName`** *(string)* **(required)** 
## B-Operation
- **`B-Operation`** *(object)*: Specifies an OpenAPI operation that is executed.
    - **`Any of (1)`**
        - **`operationId`** *(string)* **(required)** : The operationId of the OpenAPI operation.
    - **`Any of (2)`**
        - **`path`** *(string)* **(required)** : The path of the OpenAPI operation.
        - **`method`** *(string)* **(required)** : The method of the OpenAPI operation. Must be one of: `['get', 'post', 'patch', 'put', 'delete']`.

    Examples:
    ```json
    {
        "path": "/api/v1/endpoint",
        "method": "get"
    }
    ```

    ```json
    {
        "operationId": "getEndpoint"
    }
    ```

