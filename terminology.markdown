---
layout: page
title: Terminology
---

With Thuya you can easily define "structures" that can be mapped to JSON objects. To do this you need to get familiar with the following terms:

- Content Definition
- Content Field Definition
- Content Field
- Content

Let's start with an example. The following JSON contains the content of a blog post: 

```json
{
    "title": "Thuya tutorial - Part 1",
    "content": "<div>The tutorial documentation is under development.</div>",
    "publishAfter": "2024-07-21T01:24:37.000Z",
    "options": {
        "isPublic": true,
        "priority": 1
    },
    "tags": ["typescript", "tutorial", "cms"]
}
```

To handle this content in Thuya, we need to start bottom up, so let's check the properties first.

## Content field definitions

A content field definition defines the type, validation rules, and determinations for properties.

`title` is a simple string without additional validations. For this we need to create a content field definition with type text. Let's name it `common-text` as it doesn't contain any specifics.

The `content` property contains HTML data. To be secure we need to validate the HTML before save. so it requires a new content field definition with type text, containing additional validations to ensure that only strings containing valid HTML can be saved. It can be named `html-text`.

The `publishAfter` field contains a date so a content field definition with type date is required. As it is also quite generic it can be named `common-date`.

The `options` field is interesting as its value is an object and not a literal. So let's start again bottom up with its properties. `isPublic` needs a boolean while `priority` a numeric content field definition. Both are generic so the names can be `common-boolean` and `common-number`. The `options` property is a content field definition with type group. A group will not directly have a value, but can contain other fields. Groups can be nested in any depth, avoiding recursiveness. Its name can be for example `blog-post-options`.

`tags` contains an array of strings. It can use an array content field definition, for the element type reusing `common-text`. It can be called `tags`.

## Content definition

Content Definition is the top level when you define content. It is the schema of an object that uses content fields to define its structure.

The content definition for the example can be named `blog-post`. A content definition doesn't contain content field definitions directly. It contains content fields, that is a combination of a name, a content field definition and additional options. For example one content field is `title`, using content field definition `common-text` and it is required. All other content fields need to be added using the previously created content field definitions. 

## Content

The content is the data of the JSON, matching and validated by the content definition. Whenever a data is created or updated, Thuya will automatically load the corresponding content definition, remove all properties from the data that doesn't have a matching content field and validate all values using the content field definitions. Content cannot be created without a matching content definition. 