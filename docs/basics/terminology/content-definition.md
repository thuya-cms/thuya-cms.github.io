Content Definition is the top level when you define content. It is the schema of an object that uses content fields to define its structure. Each content field has a name, a content field definition and additional options.

Supported content field options are:

- **required:** cannot be saved without a value
- **indexed:** will be indexed in the database
- **unique:** cannot be saved if another content of the same content definition exists with the same value for this field
- **immutable:** a value can only be provided for create but cannot be changed afterwards

!!! note 
    Each content definition automatically contains an `id` content field. This field is unique and immutable.

## Example

The content definition for the example can be named `blog-post`. We can use the previously created [content field definitions](./content-field-definition.md#example) to add the required content fields. 

| Content definition | Content field name | Content field definition | Options           |
| ------------------ | ------------------ | ------------------------ | ----------------- |
| blog-post          |                    |                          |                   |
|                    | title              | common-text              | required, indexed |
|                    | content            | html-text                |                   |
|                    | publishAfter       | common-date              | required          |
|                    | options            | blog-post-options        |                   |
|                    | isPublic           | common-boolean           |                   |
|                    | priority           | common-number            |                   |
|                    | tags               | common-text-array        |                   |

For example one content field is `title`, using content field definition `common-text` and it is required. All other content fields need to be added using the previously created content field definitions. 

Now the the content definition is available we can create [content](./content.md) matching it.