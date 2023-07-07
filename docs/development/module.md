Extending the behavior of a Thuya application can be done using modules. Each module has metadata and can return content providers and controllers. A module can be created by extending the `Module` class.

```typescript
import { ContentProvider, Module } from "@thuya/framework";
import authContentProvider from "./auth-content-provider";
import authController from "./auth-controller";

class AuthModule extends Module {
    override getMetadata(): { name: string; } {
        return {
            name: "auth-module",
            version: 1
        };
    }

    override getContentProviders(): ContentProvider[] {
        return [commonContentProvider];
    }

    override getControllers(): IController[] {
        return [authController];
    }
}

export default new AuthModule();
```

The above example defines a module for authentication. It has metadata containing a name with the initial version. It has one content provider and one controller.

The name of a module needs to be unique in a Thuya application. The version needs to be increased for migrations. In the content providers, it is possible to define migration steps that need to be in sync with the version defined in the manifest.

!!! tip  
    How the content providers and controllers are split is up to your design. Technically it is possible to place everything into one content provider or one controller, but you can also create dedicated ones.