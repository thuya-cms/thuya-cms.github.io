Controllers make it possible to define addition actions for your application using Express router. Let's implement a simple controller that adds a `hello-world` route to our application. To do this we need a class that inherits from `IController`.

```typescript
import { IController } from "@thuya/framework";
import { Request, Response, Router } from "express";

class HelloWorldController implements IController {
    private router: Router;



    constructor() {
        this.router = Router();

        this.router.get("/hello-world", this.helloWorld);
    }
    
    
    
    getRouter(): Router {
        return this.router;
    }


    private helloWorld(request: Request, response: Response) {
        response.send("Hello world.");
    }
}

export default new HelloWorldController();
```

With this controller in place, if there is a `GET` call to the `/hello-world` url of the application, the "Hello world." text will be returned.