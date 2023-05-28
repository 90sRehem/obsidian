#typescript #zod #env #javascript 

```typescript
import { z } from 'zod'

const envVariables = z.object({
   SOMETHING_COOL: z.string()
})

envVariables.parse(process.env)

declare global {
   namespace NodeJS {
      // Property 'SOMETHING_COOL' of type 'number' is not assignable
      // to 'string' index type 'string | undefined'.
      interface ProcessEnv extends z.infer<typeof envVariables> {}
   }
}
console.log(process.env.SOMETHING_COOL)
```

resource: [link](https://twitter.com/mattpocockuk/status/1615110808219918352)