A content field definition defines the type, validation rules, and determinations for properties. Each content field definition need to have a unique name.

The following type of content field definitions can be defined:

- Text
- Numeric
- Date
- Boolean
- Array
- Group

Besides ensuring that the content will match the type these content field definitions will not validate anything additional. However new content field definitions can be created inheriting from any of these, containing more specific restrictions for the content.

## Types

### Text

Content field definition for simple text values.  
`Example: "This a a text."`

### Numeric

Content field definition for simple numeric values. Only valid numbers are accepted.  
`Example: 10`

### Date

Content field definition for simple date values. Only valid dates are accepted.  
`Example: "2024-07-21T01:24:37.000Z"`

### Boolean

Content field definition for simple boolean values. Only valid boolean values are accepted.  
`Value set: true or false`

### Array

Content field definition for an array of elements that are defined by another content field definition. For the elements content field definitions of any type are accepted.  
`Example: ["blue", "green", "red"]`

!!! tip 
    Arrays can be nested in any depth with other arrays or groups.

### Group

Content field definition containing content fields, similar to content definitions.
Sometimes we would like to define rules for not only a single value, but for the value of multiple fields. For example we would like to use date intervals in a content. It is possible to do it adding a `startDate` and an `endDate` field to the content definition and extend it with additional validations, that `startDate` is before the `endDate`. It can work. But the next time you would like to include a date range in another content definition you will need to implement the validation logic again. 

Instead it is possible to use group content fields. In this case the group would contain the `startDate` and `endDate` fields with the additional validation, that can be simply included in any content definition.

!!! tip 
    Groups can be nested in any depth with other groups or arrays.

## Example

```javascript
{
    "title": "Thuya tutorial - Part 1", // `common-text`
    "content": "<div>The tutorial documentation is under development.</div>", // `html-text`
    "publishAfter": "2024-07-21T01:24:37.000Z", // `common-date`
    "options": { // `blog-post-options`
        "isPublic": true, // `common-boolean`
        "priority": 1 // `common-number`
    },
    "tags": ["typescript", "tutorial", "cms"] // `common-text-array` with `common-text` elements
}
```

`title` is a simple string without additional validations. For this we need to create a content field definition with type text. Let's name it `common-text` as it doesn't contain any specifics.

The `content` property contains HTML data. To be secure we need to validate the HTML before save. so it requires a new content field definition with type text, containing additional validations to ensure that only strings containing valid HTML can be saved. It can be named `html-text`.

The `publishAfter` field contains a date so a content field definition with type date is required. As it is also quite generic it can be named `common-date`.

The `options` field is interesting as its value is an object and not a literal. So let's start again bottom up with its properties. `isPublic` needs a boolean while `priority` a numeric content field definition. Both are generic so the names can be `common-boolean` and `common-number`. The `options` property is a content field definition with type group. A group will not directly have a value, but can contain other fields. Groups can be nested in any depth, avoiding recursiveness. Its name can be for example `blog-post-options`.

`tags` contains an array of strings. It can use an array content field definition, for the element type reusing `common-text`. It can be called `common-text-array`.

Now that all content fields have definitions let's take a look at the next level, [content definitions](./content-definition.md).