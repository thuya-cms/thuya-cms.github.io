# Content management

Thuya is based on Express, so content can be managed using REST calls. The body of the calls is always expected to be valid JSON. You can manage the following objects:

- Content definitions
- Content field definitions
- Content

While content definitions and content field definitions are always available on the same URL, the access to content depends on the name of the corresponding content definition. But let's take a look at the details starting with [content definitions](./content-definition.md) first.