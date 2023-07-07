To create a new content definition, a class needs to be inherited from the content definition abstract class.

Let's create a content definition that represents user login data. Assume that the required content field definitions already exist.

```typescript
import { ContentDefinition } from '@thuya/framework';
import User from "./types/user";
import emailFieldDefinition from "../content-field/email-content-field-definition";
import passwordFieldDefinition from "../content-field/password-content-field-definition";

class UserContentDefinition extends ContentDefinition<User> {
    constructor() {
        super("", "user");

        this.addContentField("email", emailFieldDefinition, { isRequired: true, isUnique: true, isIndexed: true });
        this.addContentField("password", passwordFieldDefinition, { isRequired: true });
    }
}

export default new UserContentDefinition();
```

With this content definition, we can simply create users, containing an email and a password field. With proper content field definitions, it is ensured that email is valid and the password meets the security requirements and is hashed.