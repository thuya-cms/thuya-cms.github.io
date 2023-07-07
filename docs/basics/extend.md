The Thuya CMS Framework only provides the base to manage content definitions, content field definitions and content. It does not provide any authentication or authorization logic and defines only a few content that is required by the framework to operate correctly. However it is possible to extend an application.

## Modules

Extending an application can be achieved using modules. A module has a name and a version and can contain content providers and controllers to extend the application with additional content or logic.

## Content providers

Content providers are used to serve data to the Thuya CMS. Data can be content definition, content field definition and initial content. 

It also support migration from an older module version to a new one. With each version content definitions, content field definitions and initial content can be created, updated and deleted. 

!!! warning
    Content definitions and content field definitions needs to be uniquely named among all modules used by an application. 

### Controllers

Sometimes you do not want to define data as a content definition but extend the application with additional functions. Using controllers you can add any REST operation to the application. For example add an implementation to log in using a `POST` operation on path `/login`.