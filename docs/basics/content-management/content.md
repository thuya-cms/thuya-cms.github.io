!!! note 
    content definition is a prerequisite to create content.

## Schema

The schema of a content is dynamic and depends on the content definition.

| Name              |     |     | Type   | Optional | Unique | Immutable |
| ----------------- | --- | --- | ------ | -------- | ------ | --------- |
| id                |     |     | string | true     | true   | true      |
| <content fields\> |     |     |        |          |        |           | 

## Operations

### Read 

#### By id

Request: `GET <host>/<content-definition-name>/<id>`

Will return a single content of a content definition by id.

### List 

Request: `GET <host>/<content-definition-name>`

Will return all content for the content definition.

### Create

Request: `POST <host>/<content-definition-name>`

Will create a new content based on the content definition. The body should contain the content data without the `id` field.

### Update

Request: `PATCH <host>/<content-definition-name>`

Will update an existing content based on the content definition. The body should contain the content data including the `id` field. The content should contain all properties, delta update is not supported.

### Delete

#### By id

Request: `DELETE <host>/<content-definition-name>/<id>`

Will delete an existing content by id. 

