#javascript #node #programming 

For individual dirs:

```javascript
var fs = require('fs');
var dir = './tmp';

if (!fs.existsSync(dir)){
    fs.mkdirSync(dir);
}
```

Or, for nested dirs:

```javascript
var fs = require('fs');
var dir = './tmp/but/then/nested';

if (!fs.existsSync(dir)){
    fs.mkdirSync(dir, { recursive: true });
}
```