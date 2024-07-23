
If you make modifications in the backend file, and its not reflecting, then one reason could be that you need to restart your server. This is frequent chore and can be resolved by a npm package called nodemon. 

Nodemon watches any changes in your folder structure or the files of your project. Dont install it as a dependency but rather a dev-dependency. ==*Dev dependencies are those npm packages which are not going to get sent to your build files.*== 

Nodemon is just for developer convenience and hence should be installed as a dev-dependency. `-D` instructs npm to install it as a dev-dependency.

## Installation

```bash
npm i -D nodemon
```


Node: since nodemon targets everything inside the root folder, sometimes nodemon goes in a loop restarting the server in repeatedly thus causing heap memory being overflown.

To fix this you can create a `nodemon.json` file at root level and declare the files and folder that you want to watch and exclude files or folder that you don't wanna watch for changes.


## Todos

- [ ] create a nodemon.json file & define a real world example where nodemon can go into a loop of restarting the server & thus crashing.