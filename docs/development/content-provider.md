In Thuya there are not strict requirements where the files need to be located and what should be the name. You can place them wherever you want and use any name for content definitions and content field definitions. But somehow the framework needs to know that these exist. That's why we have content providers.

Content providers are used to serve data to the Thuya CMS. Data can be content definition, content field definition and initial content. To create a content provider a class has to extend the abstract `ContentProvider` class.

```typescript
import { ContentDefinition, ContentFieldDefinition, ContentProvider } from "@thuya/framework";
import authRestrictionContentDefinition from "./content-definition/auth-restriction-content-definition";
import rolesContentFieldDefinition from "./content-field/roles-content-field-definition";
import operationsContentFieldDefinition from "./content-field/operations-content-field-definition";
import contentDefinitionNameContentFieldDefinition from "./content-field/content-definition-name-content-field-definition";

class AuthContentProvider extends ContentProvider {
    override getContentFieldDefinitions(): ContentFieldDefinition[] {
        return [
            rolesContentFieldDefinition, 
            operationsContentFieldDefinition,
            contentDefinitionNameContentFieldDefinition
        ];
    }

    override getContentDefinitions(): ContentDefinition[] {
        return [authRestrictionContentDefinition];
    }

    override async getInitialContent(): { contentDefinitionName: string, content: any }[] {
        return [{
            contentDefinitionName: authRestrictionContentDefinition.getName(),
            content: {
                contentDefinitionName: authRestrictionContentDefinition.getName(),
                operations: ["POST", "GET", "PATCH", "DELETE"],
                roles: ["admin"]
            }
        }, {
            contentDefinitionName: authRestrictionContentDefinition.getName(),
            content: {
                contentDefinitionName: userContentDefinition.getName(),
                operations: ["POST", "GET", "PATCH", "DELETE"],
                roles: ["admin"]
            }
        }];
    }
}

export default new AuthContentProvider();
```

The above content provider returns content field definitions for roles, operations and content definition name. It also returns a content definition for authorization restriction. Then with the `getInitialContent` method it defines authorization restrictions that should exist when the system starts, using the previously provided content definition.

!!! note
    Content field definitions will not be extracted from content definitions. Each content field definition needs to be provided separately.

## Migrations

Sometimes you would like to update content definitions, content field definitions or initial content in a live application. This can be achieved using migrations. Let's enhance the previous example.

```typescript
import { ContentDefinition, ContentFieldDefinition, ContentProvider, MigrationOperation } from "@thuya/framework";
import authRestrictionContentDefinition from "./content-definition/auth-restriction-content-definition";
import userContentDefinition from "./content-definition/user-content-definition";
import rolesContentFieldDefinition from "./content-field/roles-content-field-definition";
import operationsContentFieldDefinition from "./content-field/operations-content-field-definition";
import contentDefinitionNameContentFieldDefinition from "./content-field/content-definition-name-content-field-definition";
import emailAddressContentFieldDefinition from "./content-field/email-address-content-field-definition";

class AuthContentProvider extends ContentProvider {
    override getContentFieldDefinitions(): ContentFieldDefinition[] {
        return [
            rolesContentFieldDefinition, 
            operationsContentFieldDefinition,
            contentDefinitionNameContentFieldDefinition,
            emailAddressContentFieldDefinition
        ];
    }

    override getContentDefinitions(): ContentDefinition[] {
        return [authRestrictionContentDefinition, userContentDefinition];
    }

    override async getInitialContent(): { contentDefinitionName: string, content: any }[] {
        return [{
            contentDefinitionName: authRestrictionContentDefinition.getName(),
            content: {
                contentDefinitionName: authRestrictionContentDefinition.getName(),
                operations: ["POST", "GET", "PATCH", "DELETE"],
                roles: ["admin"]
            }
        }, {
            contentDefinitionName: authRestrictionContentDefinition.getName(),
            content: {
                contentDefinitionName: userContentDefinition.getName(),
                operations: ["POST", "GET", "PATCH", "DELETE"],
                roles: ["admin"]
            }
        }];
    }

    override getContentFieldDefinitionMigrations(): { version: number, migration: { operation: MigrationOperation, contentFieldDefinition: ContentFieldDefinition }[] }[] {
        return [{
            version: 2,
            migration: [{
                operation: MigrationOperation.Created,
                contentFieldDefinition: emailAddressContentFieldDefinition
            }]
        }];
    }
    
    override getContentDefinitionMigrations(): { version: number, migration: { operation: MigrationOperation, contentDefinition: ContentDefinition }[] }[] {
        return [{
            version: 2,
            migration: [{
                operation: MigrationOperation.Created,
                contentDefinition: userContentDefinition
            }]
        }];
    }

    override getContentMigrations(): { version: number, migration: { operation: MigrationOperation, contentDefinitionName: string, content: any }[] }[] {
        return [{
            version: 2,
            migration: [{
                operation: MigrationOperation.Created,
                contentDefinitionName: userContentDefinition.getName(),
                content: {
                    emailAddress: "admin@mydomain.com"
                }
            }]
        }];
    }
}

export default new AuthContentProvider();
```

The code above does the following:

- Creates a new content field definition for email address.
- Creates a new content definition for user.
- Creates a new initial content for an admin user.

Each migration contains version 2. Assuming that the initial version was 1, the framework will detect that a higher version is available and execute the steps.

!!! warning
    The original data should always reflect the latest state of the application. In case of a new deployment as the metadata contains the latest version number the migrations will not be executed.