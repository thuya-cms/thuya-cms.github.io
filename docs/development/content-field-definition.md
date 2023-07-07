To create a new content field definition, a class needs to be inherited from one of the type-specific classes:

- TextContentFieldDefinition
- NumericContentFieldDefinition
- BooleanContentFieldDefinition
- DateContentFieldDefinition
- ArrayContentFieldDefinition
- GroupContentFieldDefinition

## Define a text

First, let's take a simple example of `TextContentFieldDefinition` to define a new content field definition for email addresses.

```typescript
import { TextContentFieldDefinition, Result } from "@thuya/framework";

class EmailAddressContentFieldDefinition extends TextContentFieldDefinition {
    protected filePath: string = __filename;



    constructor() {
        super("", "email-address");

        this.addValidator(this.validateEmail);
        this.addDetermination(this.convertToLowerCase);
    }



    private validateEmail(fieldValue: string): Result {
        // isValidEmail function is not implemented in the example.
        if (isValidEmail(fieldValue)) 
            return Result.error(`Invalid email address.`);

        return Result.success();
    }

    private convertToLowerCase(fieldValue: string): string {
        return fieldValue.toLowerCase();
    }
}

export default new EmailAddressContentFieldDefinition();
```

The above example will create a content field definition that will validate the input content to see if it is a valid email address. For valid input, it will convert the string to lowercase. It can be used in any content definition for email fields.

All other simple types like numeric, date, and boolean can be defined the same way. Arrays and groups are a little more complex, so let's take a look at them as well.

## Define an array

Let's assume we would like to collect the email address of users in a content, who are authorized to edit it. To achieve it we need an array of email addresses. Luckily we already have the content field definition for email addresses, so only the array content field definition is required.

```typescript 
import { ArrayContentFieldDefinition } from "@thuya/framework";
import { emailAddressContentFieldDefinition } from "./email-address-content-field-definition";

class EmailAddressesContentFieldDefinition extends ArrayContentFieldDefinition {
    constructor() {
        super("", "email-addresses", emailAddressContentFieldDefinition);
    }
}

export default new EmailAddressesContentFieldDefinition();
```

And that's all. With this content field definition, we have an array of email addresses. All of the elements will be validated and converted to lowercase as defined in the email address content field definition.

## Define a group

Date ranges are used frequently. You could define a simple data content field definition and them as a start date and an end date to each content definition, but you would need to implement interval validations in each content definition separately. That doesn't sound too good.

Instead, we can create a content field definition that contains multiple fields. This is called a group. As it is a content field definition it can contain validations and determinations as usual, so the interval check can be implemented. Then this content field definition can be used in any content definition and it is guaranteed that the intervals are always valid.

For simplicity reasons let's assume that content field definition for a simple date already exists.

```typescript
import { GroupContentFieldDefinition, Result } from "@thuya/framework";
import { commonDateContentFieldDefinition } from "./common-date-content-field-definition";

type DateInterval = {
    startDate: Date,
    endDate: Date
}

class DateIntervalContentFieldDefinition extends GroupContentFieldDefinition<DateInterval> {
    protected filePath: string = __filename;
    
    
    
    constructor() {
        super("", "date-interval");

        this.addContentField("startDate", commonDateContentFieldDefinition, { isRequired: true });
        this.addContentField("endDate", commonDateContentFieldDefinition, { isRequired: true });

        this.addValidator(this.validateInterval);
    }



    private validateInterval(fieldValue: DateInterval): Result {
        if (fieldValue.endDate < fieldValue.startDate)
            return Result.error(`Invalid date interval.`);

        return Result.success();
    }
}

export default new DateIntervalContentFieldDefinition();
```

The above definition will validate if the start date is before the end date. When adding the content fields to the group it is also set that both fields are required, so it is not possible to create a date interval if any of its fields is missing.

!!! tip "Nesting"
    Groups and arrays can be nested in any depth. So arrays can contain groups or arrays and groups can also contain other groups or arrays.