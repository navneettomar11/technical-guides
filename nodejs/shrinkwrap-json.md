# package.json
I am sure if you are using npm(Node package Manager) then you already know about the versioning file i.e. **package.json**.
- It is used to install different open source and other available packages(that is pre-packaged code modules) in a Node.js project. So I am going to explain the use of it in depth.
- It us used for than dependencies - like defining project properties, description, author & license information, srcipts, etc.
- It records the minimum version you app needs. If you update the versions of a particular package, the change is not going to be reflected here.

`^` sign before the version tells npm that if someone clones the project and run npm install in the directory then install the latest minor version of the package in his node_modules.

So lets say I am having express with `^3.0.0` in package.json and then express team releases version `3.5.2` and now when someone clone repo and runs `npm install` in that directory they will get the version `3.5.2`. (You can also put `~` instead of `^` it will update to latest patch version).

## Difference between tile(~) and caret(^) in package.json
If you have updated your npm to the latest version and tried installing the package using npm install moment - save you will se the see the package moment.js get saved in package.json with a caret(^) prefix and in the previous version it was saved with tilde(~) prefix. 

The tilde prefix simply indicates that the tilde (~) symbol will match the most recent patch version or the most recent minor version i.e. the middle number. For example, ~1.2.3 will match all 1.2.x versions but it will not match `1.3.0` or `1.3.x` versions.

The caret indicates the first number i.e. the most recent major version. An example is in 1.x.x release the caret will update you and it will match with 1.3.0 but not 2.0.0.

However, this can be a huge issue if package developers break any of the functions on the minor version as it can make your application break down.

So npm later released a new file called package-lock.json to avoid such scenarios.

# package-lock.json
Before we get into the details, if you wish to follow along with your own project there is one thing to check first. If you are not using the current version of npm or it is lower than 5.0.0 you'll need to update it to 5.x.x or current stable version. Once done, install the package by running the command:

> npm install 

OR

> npm i

Once this activity is done, you can see that a new file named `package-lock.json` is automatically created. If you open and read the content of this file you will find dependencies like package.json but with more details.

## Why is package-lock.json created ?
When you install any package in your project by executing the command

> npm i <package-name> --save

it will install the exact latest version of that package in your project and save the dependency in package.json with a caret(^) sign. Like if the current version of a package is 5.2.3 then the installed version will be 5.2.3 and the saved dependency will be ^5.2.3. Caret(^) means it will support any higher version with major version 5 like 5.3.1 and so on. Here, **package-lock.json is created for locking the dependency** with the installed version.

The `package-lock.json`
- is an extremely important file that is there to save you  from a lot of **boom boom bam bam** in your repositories.
- will simply avoid this general behavior of installing updated minor version so when someone clones your repo and run npm install in their machine. NPM will look into package-lock.json and install exact version of the package as the owner has installed so it will ignore the `^` and `~` from package.json.
- is solely used to lock dependencies to a specific version number.
- records the exact version of each installed package which allows you to re-install them. Future installs will be able to build an identical dependency tree.
- Also, it contains some other meta information which save time of fetching that data from npm while you do npm install.
- Describe a single representation of a dependency tree such that teammates, deployments and continuous integration are guaranteed to install exactly the same dependencies.
- Provide a facility for users to "time-travel" to previous state of `node_modules` without having to commit the directory itself.
- To facilitate greater visibility of tree change through readable source control diffs.
- And optimize the installation process by allowing npm to skip repeated metadata resoultions for previously-installed pakages.

## What is the purpose or use of package-lock.json ?
To avoid differences in installed dependencies on different environments and to generate the same result on every environment we should use the **package-lock.json** file to install dependencies.

Ideally, this file should be on your source control with the package.json file so when you or any other user will clone the project and run the command "npm i", it will install the exact same version saved in package-lock.json file and you will able to generate the same results as you developed with that particular package.

# npm-shrinkwrap.json

    