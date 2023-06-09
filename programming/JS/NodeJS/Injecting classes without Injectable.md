#node #nestjs #js #programming 

discussion link: [stackoverflow](https://stackoverflow.com/questions/69322616/nestjs-injecting-classes-without-injectable)
Try using [factory providers](https://docs.nestjs.com/fundamentals/custom-providers#factory-providers-usefactory):
example:
```javascript
import { MyClass } from '@my-library/my-class';
import { MyClassDep } from '@my-library/my-class';

function makeInjectable<T>(target: Type<T>): Type<T> {
  return {
    provide: target.constructor.name,
    useFactory: () => new target(),
  };
}

@Module({
  controllers: [],
  providers: [
    makeInjectable(myClass),
    makeInjectable(myClassDep),
  ],
})
export class UsersModule {}
```

OpenAI [link](https://chat.openai.com/chat)

In NestJS, you can use factory providers to create a service or value that is generated by a function. This can be useful if you want to dynamically generate a service or value based on certain conditions, or if you want to configure the service or value in some way before it is injected into your application.

To use a factory provider, you will need to use the `@Injectable()` decorator and the `@Inject()` decorator. You will also need to provide a function that will be used to generate the service or value.

Here is an example of how to use a factory provider to create a service:
```javascript
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class MyService {
  constructor(@Inject('Config') config: any) {
    console.log(config);
  }
}

export const myServiceFactory = {
  provide: 'MyService',
  useFactory: (config: any) => {
    return new MyService(config);
  },
  inject: ['Config'],
};
```

To use the factory provider, you will need to register it in your module's providers array. For example:
```javascript
import { Module } from '@nestjs/common';
import { MyService, myServiceFactory } from './my.service';

@Module({
  providers: [myServiceFactory],
})
export class AppModule {}

```

The factory provider function will be called whenever a new instance of `MyService` is needed, and the `inject` array specifies the dependencies that should be injected into the factory function.

I hope this helps! Let me know if you have any questions.