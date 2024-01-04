#nestjs #programming #javascript 

[Github issue](https://github.com/nestjs/nest/issues/1788)

No arquivo main
```typescript

import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { NestExpressApplication } from '@nestjs/platform-express';

async function bootstrap() {
const app = await NestFactory.create<NestExpressApplication>(AppModule);
app.getHttpAdapter().getInstance().disable('x-powered-by');
await app.listen(3000);
}

bootstrap();

```