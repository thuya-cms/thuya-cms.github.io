## Schema

Content field definitions have the following structure:

| Name                       |                       |            | Type    | Optional | Unique |
| -------------------------- | --------------------- | ---------- | ------- | -------- | ------ |
| id                         |                       |            | string  | true     | true   |
| name                       |                       |            | string  |          | true   |
| [type](#type)              |                       |            | enum    |          |        |
| arrayElementDefinitionName |                       |            | string  | true     |        |
| groupElements              |                       | array      |         | true     |        |
|                            | contentDefinitionName |            | string  |          |        |
|                            | name                  |            | string  |          |        |
|                            | options               |            |         |          |        |
|                            |                       | isRequired | boolean |          |        |

### `type`

The following values are valid:

- "numeric"
- "text"
- "date"
- "boolean"
- "array"
- "group"

## Operations

### Read 

#### By name

Request: `GET <host>/content-field-definition/name/<content field definition name>`

Will return a single content field definition by name.

### List 

Request: `GET <host>/content-field-definition`

Will return all content field definitions.

### Create

Request: `POST <host>/content-field-definition`

Will create a new content field definition. The body should contain the content field definition data without the `id` field.

### Delete

#### By name

Request: `DELETE <host>/content-field-definition/name/<content field definition name>`

Will delete an existing content field definition by name. 