
## 1. Improve typescript readability with `js-doc`

### Step - 1

Note: For this to work you need to add `"checkJs": true` in you `tsconfig.json` file. Something like this ->

`tsconfig.json` file content ->

```json
{
  "compilerOptions": {
    "checkJs": true,
	// your other parameters
  },
}

```

`index.js` file content ->

```typescript
import express, { Request, Response } from 'express';
const app = express();

/**
 * Users route.
 * @param {Request} req - Express request object
 * @param {Response} res - Express response object
 * @returns {void}
 */
app.get('/get-users', (req, res) => {
  res.status(404).send("Not found");
});

```

This approach is way better than this mess ->

```javascript
import express, { Request, Response } from 'express';
const app = express();

app.get('/get-users', (req: Request, res: Response): void => {
  res.status(404).send("Not found");
});
```

### Step - 2

Now install a `vs-code theme` which provides you a contrast in syntax. 

Combining `js-doc` with a `vs-code theme` that provides enough contrast b/w syntaxes will give you something like this screenshot.

![[Pasted image 20240724001137.png]]

As you can see the `comment` section is dimmer than the actual code and doesn't snatch your focus from the actual code.


Todo: 
- [ ] verify this doc