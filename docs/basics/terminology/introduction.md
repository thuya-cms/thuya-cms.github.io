# Terminology

With Thuya you can easily define "structures" that can be mapped to JSON objects. To do this you need to get familiar with the following terms:

- Content Field Definition
- Content Definition
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

To handle this content in Thuya, we need to start bottom up, so let's check the properties first. For each property we need to define its type. In Thuya this is defined by [content field definitions](./content-field-definition.md).