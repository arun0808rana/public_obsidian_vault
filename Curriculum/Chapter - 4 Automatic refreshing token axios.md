
```javascript
axiosInstance.interceptors.request.use(
  async (config) => {
    const token = store.getState().user.accessToken;
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
      // decode the token and check if (token.exp < Date.now() / 1000)
      if (isTokenExpired(token)) {
          // Call your refresh token endpoint
          // you have to implement an endpoint to get the refresh token
          // if you use 3rd party auth servers, you make requests to their /refreh enpoints
          const newToken = await refreshToken();
          if (newToken) {
            // Token refreshed, update the Authorization header with the new token
            config.headers.Authorization = `Bearer ${newToken}`;
            // Retry the original request
          }    
        } 
      }
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

```

todo: 

modify the above code according to this
https://stackoverflow.com/questions/51646853/automating-access-token-refreshing-via-interceptors-in-axios?rq=1

so that infinite loop condition can be avoided


```javascript
/**
 * Wrap the interceptor in a function, so that it can be re-instantiated
 */
function createAxiosResponseInterceptor() {
    const interceptor = axios.interceptors.response.use(
        (response) => response,
        (error) => {
            // Reject promise if usual error
            if (error.response.status !== 401) {
                return Promise.reject(error);
            }

            /*
             * When response code is 401, try to refresh the token.
             * Eject the interceptor so it doesn't loop in case
             * token refresh causes the 401 response.
             *
             * Must be re-attached later on or the token refresh will only happen once
             */
            axios.interceptors.response.eject(interceptor);

            return axios
                .post("/api/refresh_token", {
                    refresh_token: this._getToken("refresh_token"),
                })
                .then((response) => {

                    saveToken();
                    error.response.config.headers["Authorization"] =
                        "Bearer " + response.data.access_token;
                    // Retry the initial call, but with the updated token in the headers. 
                    // Resolves the promise if successful
                    return axios(error.response.config);
                })
                .catch((error2) => {
                    // Retry failed, clean up and reject the promise
                    destroyToken();
                    this.router.push("/login");
                    return Promise.reject(error2);
                })
                .finally(createAxiosResponseInterceptor); // Re-attach the interceptor by running the method
        }
    );
}
createAxiosResponseInterceptor(); // Execute the method once during start

```