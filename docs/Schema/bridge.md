# RIDAGOP toolkit Bridge-Schema

*Describes the capabilities.*

## Properties

- **`execute`** *(object)* **(required)** 
- **`capabilities`** *(object)* **(required)** 
  - **Additional Properties** *(object)*
    - **`operation`**: Refer to *[B-Operation](#b-operation)*.
    - **`functionName`** *(string)*
    - **`postProcess`** *(string)*
- **`errorMessagePath`** *(string)*
- **`supportModuleUri`** *(string/format: uri)*
- **`uri`** *(string)*
## Definitions

<h2 id="b-operation">B-Operation</h2>
- **`B-Operation`** *(object)*
  - **`path`** *(string/format: json-pointer)*
  - **`method`** *(string)*: Must be one of: `['get', 'post', 'patch', 'put', 'delete']`.
  - **`operationId`** *(string)*

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

