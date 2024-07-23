
`route.js` ->

```javacript
app.get('/user',
rbac({
	currentRoute: '/user',
	operationTypes: ['view', 'delete']
}),
route);
```

`rbac.js` ->

```javascript
function rbac({ operationTypes, currentRoute }) {
  return async (req, res, next) => {
    const response = {
      success: true,
      error: null,
    };
    try {
      if (!operationTypes?.length) {
        throw new Error("Please provide operation type");
      }

      const permissions = await getPersmissions();

      // get the permission against which the rbac should be performed
      // This involes checking the current role's previleges pre-defined in the permissions array
      const targetRoutePermission = permissions.find(
        (permission) => permission.route === currentRoute
      );
      if (!targetRoutePermission?.operationTypes) {
        throw new Error(
          "Cannot find accessable operation types for the current route"
        );
      }
      const permissionOperationTypes = targetRoutePermission.operationTypes;

      // throw error if the current route is not found
      if (!targetRoutePermission?.route) {
        throw new Error("No permission set for the current route");
      }

      //check if the role has permisson to access the current route
      // as well as check if it has access but satisfies the type of access(view, edit,delte,etc)
      for (const incomingOperationType of operationTypes) {
        if (!permissionOperationTypes.includes(incomingOperationType)) {
          throw new Error("User is not authorized for this action");
        }
      }

      next();
    } catch (error) {
      console.error("Error in rbac middleware", error.message);
      (response.success = false), (response.error = error.message);
      return res.json(response);
    }
  };
}

async function getPersmissions() {
  return [
    {
      route: "/users",
      accessTo: ["admin", "super_admin"],
      operationTypes: ["view", "edit"],
    },
    {
      route: "/posts",
      accessTo: ["admin", "super_admin"],
      operationTypes: ["delete", "edit"],
    },
  ];
}

```


