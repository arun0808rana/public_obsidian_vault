
# 1. Improve typescript readability with `js-doc`

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

# 2. `ts-node` vs `tsc`


Both `tsc` and `ts-node` are tools used for working with TypeScript, but they serve different purposes and are used in different scenarios. Here's a comparison of the two:

## TypeScript Compiler (tsc)

**Purpose:**
- `tsc` is the official TypeScript compiler provided by Microsoft.
- It is used to compile TypeScript code (.ts files) into JavaScript code (.js files).

**Usage:**
- Typically used in the build process to convert TypeScript code into JavaScript before deployment.
- Generates JavaScript files that can be run in any JavaScript environment (Node.js, browsers, etc.).

**Features:**
- Checks for type errors and ensures type safety before generating JavaScript code.
- Supports advanced TypeScript features like decorators, enums, and generics.
- Allows configuration through `tsconfig.json` for specifying compiler options, file inclusions, and exclusions.

**Advantages:**
- Generates standalone JavaScript files that can be deployed and run independently.
- Ensures type safety and error checking before running the code.
- Can produce different module formats (CommonJS, ES6 modules, etc.) based on the configuration.

## ts-node

**Purpose:**
- `ts-node` is a TypeScript execution environment for Node.js.
- It allows you to run TypeScript code directly without needing to compile it to JavaScript first.

**Usage:**
- Commonly used in development for running TypeScript scripts, development servers, and testing.
- Ideal for quick prototyping and iterative development.

**Features:**
- Executes TypeScript files directly using Node.js, bypassing the need for a separate compilation step.
- Integrates with `tsconfig.json` for consistent compiler options.
- Supports `REPL` (Read-Eval-Print Loop) for running TypeScript commands interactively.

**Advantages:**
- Faster iteration during development since there is no need to compile TypeScript to JavaScript separately.
- Simplifies running TypeScript scripts and development servers.
- Useful for testing and debugging TypeScript code directly.

## Comparison

| Feature                | tsc                       | ts-node                     |
|------------------------|---------------------------|-----------------------------|
| Compilation            | Compiles TypeScript to JavaScript | Executes TypeScript directly     |
| Use Case               | Build process, deployment | Development, testing, prototyping |
| Output                 | Generates .js files       | No separate output files         |
| Type Checking          | Yes                       | Yes                           |
| Configuration          | `tsconfig.json`           | `tsconfig.json`               |
| Performance            | Slower due to separate compile step | Faster for development             |
| Usage in Production    | Yes                       | Typically no                  |

## Conclusion

- **Use `tsc`** when you need to compile TypeScript code into JavaScript for deployment and production use. It ensures type safety and produces standalone JavaScript files.
- **Use `ts-node`** for development, testing, and quick prototyping where you want to run TypeScript code directly without the overhead of a separate compilation step.

Choosing between the two depends on the stage of development and the specific needs of your project. For a typical workflow, you might use `ts-node` during development and `tsc` for building your project for production.





Todo: 
- [ ] verify this doc