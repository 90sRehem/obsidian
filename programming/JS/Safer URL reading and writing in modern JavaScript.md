#js #javascript  

[link](https://www.builder.io/blog/new-url#common-issue-2-forgetting-to-encode)
## You might unknowingly be writing URLs in an unsafe way

Can you spot the bug in this code?

```javascript
const url = `https://builder.io/api/v2/content
  ?model=${model}&locale=${locale}?query.text=${text}`

const res = await fetch(url)
```

There are at _least_ three!

We will break them down below:

## Common issue #1: incorrect separator characters

![A URL string with an extra `?`](https://cdn.builder.io/api/v1/image/assets%2FYJIGb4i01jvw0SRdL5Bt%2Fc6854e9c731345b49a5d57aa49cf9d66?width=800)

Oops! This is certainly a newbie mistake, but one so easy to miss I’ve caught this in my own code even after 10 years of JS development.

A common culprit for this in my experience is after editing or moving code. For example, you have a correctly structured URL, then copy one piece from one to another, and then miss that the param separator was wrongly ordered.

This can also happen when concatenating. For instance:

```javascript
url = url + '?foo=bar'
```

But wait, the original `url` may have had a query param in it. Ok, so this should be:

```javascript
url = url + '&foo=bar'
```

But wait, if the original `url` _didn’t_ have query params then this is now wrong. Argh.

## Common issue #2: forgetting to encode

![A URL string with a param without encoding](https://cdn.builder.io/api/v1/image/assets%2FYJIGb4i01jvw0SRdL5Bt%2F316c6281fdf046d79a99498cd031a5e1?width=800)

Gah. `model` and `locale` likely don’t need to be encoded, as they are URL-safe values, but I didn’t stop to think `text` can be all kind of text, including whitespace and special characters, which will cause us problems.

So maybe we’ll overcorrect and play things extra safe:

```javascript
const url = `https://builder.io/api/v2/content
  ?model=${
    encodeURIComponent(model)
  }&locale=${
    encodeURIComponent(locale)
  }&query.text=${
    encodeURIComponent(text)
  }`
```

But things are feeling a little…uglier.
## Common issue #3: accidental whitespace characters


![A URL string with accidental whitespace characters](https://cdn.builder.io/api/v1/image/assets%2FYJIGb4i01jvw0SRdL5Bt%2F8aebf9b5f0e14784a21a1e712f986efa?width=800)

Oof. In order to break this long URL into multiple lines, we accidentally included the newline character and extra spaces into the URL, which will make fetching this no longer work as expected.

We can break the string up properly now, but we’re getting even messier and harder to read:

```javascript
const url = `https://builder.io/api/v2/content`
  + `?model=${
    encodeURIComponent(model)
  }&locale=${
    encodeURIComponent(locale)
  }&query.text=${
    encodeURIComponent(text)
  }`
```

That was a lot just to make constructing one URL correct. And are we going to remember all this next time, especially as that deadline is rapidly approaching and we need to ship that new feature or fix asap?

There has to be a better way.

![A gif of Joey from friends saying "There's gotta be a better way!"](https://cdn.builder.io/o/assets%2FYJIGb4i01jvw0SRdL5Bt%2F581b6ab4fe124a5bb6793f3d55e72b9f%2Fcompressed?apiKey=YJIGb4i01jvw0SRdL5Bt&token=581b6ab4fe124a5bb6793f3d55e72b9f&alt=media&optimized=true)

## The `URL` constructor to the rescue

A cleaner and safer solution to the above challenge is to use the [URL constructor](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL):

```javascript
const url = new URL('https://builder.io/api/v2/content')

url.searchParams.set('model', model)
url.searchParams.set('locale', locale)
url.searchParams.set('text', text)
  
const res = await fetch(url.toString())
```

This solves several things for us:

- Separator characters are always correct (`?` for the first param, and thereafter).
- All params are automatically encoded.
- No risk of additional whitespace chars when breaking across multiple lines for long URLs.

## Modifying URLs

It is also incredibly helpful for situations where we are modifying a URL but we don’t know the current state.

For instance, instead of having this issue:

```javascript
url += (url.includes('?') ? '&' : '?') + 'foo=bar'
```

We can instead just do:

```javascript
// Assuming `url` is a URL
url.searchParams.set('foo', 'bar')

// Or if URL is a string
const structuredUrl = new URL(url)
structuredUrl.searchParams.set('foo', 'bar')
url = structuredUrl.toString()
```

Similarly, you can also write other parts of the URL:

```javascript
const url = new URL('https://builder.io')

url.pathname = '/blog'      // Update the path
url.hash = '#featured'      // Update the hash
url.host = 'www.builder.io' // Update the host

url.toString()              // https://www.builder.io/blog#featured
```

## Reading URL values

Now, the age-old problem of “I just want to read a query param from the current URL without a library” is solved.

```javascript
const pageParam = new URL(location.href).searchParams.get('page')
```

Or for instance update the current URL with:

```javascript
const url = new URL(location.href)
const currentPage = Number(url.searchParams.get('page'))
url.searchParams.set('page', String(currentPage + 1))
location.href = url.toString()
```

But this is not just limited to the browser. It can also be used in Node.js

```javascript
const http = require('node:http');

const server = http.createServer((req, res) => {
  const url = new URL(req.url, `https://${req.headers.host}`)
  // Read path, query, etc...
});
```

As well as Deno:

```javascript
import { serve } from "https://deno.land/std/http/mod.ts";
async function reqHandler(req: Request) {
  const url = new URL(req.url)
  // Read path, query, etc...
  return new Response();
}
serve(reqHandler, { port: 8000 });
```

## URL properties to know

URL instances support all of the properties you are already used to in the browser, such as on `window.location` or anchor elements, all of which you can both read _and_ write:

```javascript
const url = new URL('https://builder.io/blog?page=1');

url.protocol // https:
url.host     // builder.io
url.pathname // /blog
url.search   // ?page=1
url.href     // https://builder.io/blog?page=1
url.origin   // https://builder.io
url.searchParams.get('page') // 1
```

Or, at a glance:

![A diagram of a URL and arrows pointing to each segment such as where the "hostname" vs "hash" and so on ≠are.](https://cdn.builder.io/api/v1/image/assets%2FYJIGb4i01jvw0SRdL5Bt%2F09be949f4a4843c7abde1b25c0c2636c?width=800)

## URLSearchParams methods to know

The `URLSearchParams` object, accessible on a `URL` instance as `url.searchParams` supports a number of handy methods:

### `searchParams.has(name)`

Check if the search params contain a given name:

```javascript
url.searchParams.has('page') // true
```

### `searchParams.get(name)`

Get the value of a given param:

```javascript
url.searchParams.get('page') // '1'
```

### `searchParams.getAll(name)`

Get all values provided for a param. This is handy if you allow multiple values at the same name, like `&page=1&page=2`:

```javascript
url.searchParams.getAll('page') // ['1']
```

### `searchParams.set(name, value)`

Set the value of a param:

```javascript
url.searchParams.set('page', '1')
```

### `searchParams.append(name, value)`

Append a param — useful if you potentially support the same param multiple times, like `&page=1&page=2`:

```javascript
url.searchParams.append('page', '2')
```

### `searchParams.delete(name)`

Remove a param from the URL entirely:

```javascript
url.searchParams.delete('page')
```

## Pitfalls

The one big pitfall to know is that all URLs passed to the URL constructor must be absolute.

For instance, this will throw an error:

```javascript
new URL('/blog') // ERROR!
```

You can resolve that, by providing an origin as the second argument, like so:

```javascript
new URL('/blog', 'https://builder.io')
```

Or, if you truly need to only work with URL parts, you could alternatively use [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) directly if you just need to work with query params of a relative URL:

```javascript
const params = new URLSearchParams('page=1')
params.set('page=2')
params.toString()
```

`URLSearchParams` has one other nicety as well, which is that it can take an object of key value pairs as its input as well:

```javascript
const params = new URLSearchParams({
  page: 1,
  text: 'foobar',
})
params.set('page=2')
params.toString()
```

## Browser and runtime support

`new URL` supports all modern browsers, as well as Node.js and Deno! ([source](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL))

![A table of browser support - which you can get to in the "source" link above.](https://cdn.builder.io/api/v1/image/assets%2FYJIGb4i01jvw0SRdL5Bt%2F4ed6b279ae1d4f8aa4a790fe5484517c?width=800)