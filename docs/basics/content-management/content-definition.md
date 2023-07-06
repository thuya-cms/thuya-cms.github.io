## Schema

Content definitions have the following structure:

| Name          |                                                                        |            | Type    | Optional | Unique |
| ------------- | ---------------------------------------------------------------------- | ---------- | ------- | -------- | ------ |
| id            |                                                                        |            | string  | true     | true   |
| name          |                                                                        |            | string  |          | true   |
| contentFields |                                                                        | array      |         |          |        |
|               | [name](#contentfieldsname)                                             |            | string  |          |        |
|               | [contentFieldDefinitionName](#contentfieldscontentfielddefinitionname) |            | string  |          |        |
|               | options                                                                |            |         |          |        |
|               |                                                                        | isRequired | boolean |          |        |
|               |                                                                        | isUnique   | boolean |          |        |
|               |                                                                        | isIndexed  | boolean |          |        |

### `contentFields.name`

Needs to be unique within a content definition.

### `contentFields.contentFieldDefinitionName`

Needs to match the name of an existing content field definition.

## Operations

### Read 

#### By name

Request: `GET <host>/content-definition/name/<content definition name>`

Will return a single content definition by name.

### List 

Request: `GET <host>/content-definition`

Will return all content definitions.

### Create

Request: `POST <host>/content-definition`

Will create a new content definition. The body should contain the content definition data without the `id` field.

!!! warning
    Before creating a content definition you should ensure that all [content field definitions exist](./content-field-definition.md).

### Update

Request: `PATCH <host>/content-definition`

Will update an existing content definition. The body should contain the content definition data including the `id` field. The content should contain all properties, delta update is not supported.

!!! warning
    Before updating a content definition you should ensure that all [content field definitions exist](./content-field-definition.md).

### Delete

#### By name

Request: `DELETE <host>/content-definition/name/<content definition name>`

Will delete an existing content definition by name. 