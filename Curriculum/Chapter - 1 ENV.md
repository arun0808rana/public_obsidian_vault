
## 1. Problem

If you find yourself writing `axios.get('http://localhost:8000/route_name)` 10 times because you have 10 different files that require this URL then there is an alternative to resolve this tedious task.


Why?

Because in future you will be required to change this URL. Hence making modifications in 10 different files doesn't make sense.


## 2. Solution: 

1. Create a `.env` file at root level of your project.
2. Create key-value pairs
3. For example

```
BASE_URL=http://localhost:8000
```

Using this file now you can replace the `axios.get('http://localhost:8000/route_name)` with the following

``` javascript
const BASE_URL = process.env.BASE_URL;
axios.get(`${BASE_URL}/route_name`)
```


Note: `process.env.YOUR_ENV_VARIABLE_NAME` is a way of reading .env file's variables in you javascript files. This works for javascript based frameworks as well such react.js, angular.js, vue.js, etc.

If for some reasons you need modification in baseurl then all you need is to change the value or BASE_URL in the .env file and all your 10 files will reflect that change automatically


## 3. Segregating the environment

Reason -> Lets say if you have different db urls and credentials for localhost & production.
If you find yourself modifying the .env file for managing different environments(local or dev or prod) then follow the below alternative. This will save your time and effort

### a.  Pre-requisite for this solution

First install a package called `env-cmd`

```bash
npm i env-cmd
```

### b. Create a file for each enviroment

```bash
touch .env.local # This is for your localhost testing and development
touch .env.prod # This is for your production
```

### c. Populate these files with the env variables

`.env.loca` file content ->

```
BASE_URL=http://locahost:8000

MY_SQL_USERNAME=root
MY_SQL_PASSWORD=admin
```

`.env.prod` file content ->

```
BASE_URL=https://live-website.com

MY_SQL_USERNAME=production_sql_user_name
MY_SQL_PASSWORD=very_strong_password
```

### d. Integration in `package.json` file

In the `scripts` section of your `package.json` file create 2 entries ->

```json
{

  "scripts": {
    "local": "env-cmd -f .env.local nodemon index.js",
    "prod": "env-cmd -f .env.local node index.js"
  }

}
```

Now all you have to do while working with different environments is run separate commands and `env-cmd` will automatically pick the right .env file for you. Lets say you can run `npm run local` on localhost & `npm run prod` on production server & `env-cmd` will automatically choose the correct .env file.

```bash
npm run local # for locahost

#or

npm run prod # for prod
```


## Advance usage guide

https://www.npmjs.com/package/env-cmd#-advanced-usage
