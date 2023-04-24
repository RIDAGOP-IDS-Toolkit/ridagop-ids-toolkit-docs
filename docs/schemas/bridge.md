# RIDAGOP toolkit Bridge-Schema

*Describes the capabilities.*

## Properties

- **`execute`** *(object)* **(required)** : Defines the type of execution. Either by using an OpenAPI-Specification or by using a client library.
    - **`One of (1)`**: OpenAPI-Specification.
        - **`openapiSchemaUri`** *(string/format: uri-reference)* **(required)** : The URI of the OpenAPI-Specification.
    - **`One of (2)`**: Client library module.
        - **`apiClientModuleUri`** *(string/format: uri-reference)* **(required)** : The URI of the module that contains the client library.
- **`capabilities`** *(object)* **(required)** : Defines the capabilities of the bridge.
    - **Additional Properties** *(object)*: Each name defines a capability. The allowed names are defined in the capabilities_names_list.json file.
        - **`operation`**: Specifies an OpenAPI operation that should be executed. Refer to *[B-Operation](#b-operation)*.
        - **`functionName`** *(string)*: Specifies the name of the function (in case of a client library).
- **`errorMessagePath`** *(string)*: Relative json-path in the response, where an error-message is located, if the response is not a 2XX answer.
- **`supportModuleUri`** *(string/format: uri)*: The URI of the module that contains the support functions.
## Definitions

## B-Operation
- **`B-Operation`** *(object)*: Specifies an OpenAPI operation that is executed.
    - **`path`** *(string)*: The path of the OpenAPI operation.
    - **`method`** *(string)*: The method of the OpenAPI operation. Must be one of: `['get', 'post', 'patch', 'put', 'delete']`.
    - **`operationId`** *(string)*: The operationId of the OpenAPI operation.

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

