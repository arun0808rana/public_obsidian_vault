## Installation

```bash
npm install axios
```



## Usage

```javascript
import axios from 'axios';

axios.get('https://jsonplaceholder.typicode.com/todos/1')
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.log(error);
  })
```

Note: The actual data is inside the `.data` key of response object.




## Common usage

In production, the above code is written in the following way.

```javascript
import axios from 'axios';

const axiosInstance = axios.create({
  baseURL: 'https://jsonplaceholder.typicode.com',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});

axiosInstance.get('/todos/1')
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.log(error);
  })
```

You have to create a single instance of axios. Reason --

Lets say you have to send a HTTP request header called `X-Custom-Header: 'foobar'` in each api call. If you use axios.get() instead of axiosInstance.get() then each time you will have to manually attach the header in each request.

Creating an instance gives you the freedom to attach some common data to each request without actually attaching it multiple times manually.