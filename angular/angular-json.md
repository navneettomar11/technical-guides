# Angular Workspace Configuration
A file named *angular.json* at the root level of an Angular Workspace provides workspace-wide and project specific configuration defaults for build and development tools provided by the Angular CLI. Path values given in the configuration are relative to the root workspace folder.

## Overall JSON structure
At the top level of _angular.json_, a few properties configure the workspace, and a projects section contains the remaining per-project configuration options. CLI defaults set at the workspace level can be overridden by defaults set at the project level, and defaults set at the project level can be overridden on the command line.
The following properties, at the top level of the file, configure the workspace.
- **version**: The configure-file version.
- **newProjectRoot**: Path where new projects are created. Absolute or relative to the workspace folder.
- **defaultProject**: Default project name to use in commands, where not provided as an argument. When you use `ng new` to create a new app in a new workspace, that app is the default project for workspace until you change it here.
- **schematics**: A set of schematics that customize that `ng generate` sub-command option defaults for this workspace.
- **project**: Contains a subsection for each project (library or application) in the workspace, with the per project configuration options.
The initial app that you create with `ng new app_name` is listed under 'projects'
```json
"projects": {
  "app_name": {
    ...
  }
  ...
}
```
Each additional app that you create with `ng generate` application has a corresponding end-to-end test project, with its own configuration section. When you create a library, the library project is also added to the `projects` subsection

## Project configuration options
The following to-level configuration properties are available for each project, under projects:<project_name>
```json
"my-app" : {
  "root": "",
  "sourceRoot": "src",
  "projectType": "application",
  "prefix": "app",
  "schematics": {},
  "architect": {}
}
```  
|Property|Description|
|root| The root folder for this project's files, relative to the workspace folder. Empty for the initial app, which resides at the top level of the workspace.|
|sourceRoot| The root folder for this project's source files.|
|projectType| One of "application" or "library". An application can run independently in a browser, while a library cannot.|
|prefix| A string that Angular prepends to generated selectors. Can be customized to identify an app or feature.|
|schematics| A set of schematics that customize the `ng generate` sub command option defaults for this project.|
|architect| Configuration defaults for Architect builder target for this project|

## Generate schematics
Angular generation schematics are instructions for modifying a project by adding files or modifying existing files. Individual schematics for the default Angular CLI `ng generate` sub-commands are collected in the package @angular. Specify the schematics name for a subcommand in the format `schematics-package:schematic-name`; for example, the schematic for generating a component is @angular:component.

The JSON schemas for the default schematics used by the CLI to generate projects and parts of projects are collected in package @schematics/angular. The schema describe the option available to the CLI for each of the ng generate subcommand as shown in the --help output.

The field given in the schema correspond to the allowed argument values and defaults for the CLI sub-command options. You can update your workspace schema file to set a different default for subcommand option.

## Project toll configuration options
Architect is the tool that the CLI uses to perform complex tasks, such as compilation and testing running. Architect is a shell that runs a specified builder to perform a given task, according to a target configuration. You can define and configure. You can define and configure new builders and targets to extend the CLI.

### Default Architect builders and targets
Angular defines default builders for use with specific CLI commands, or with the general `ng run` command. The JSON schemas that the define the options and default for each of these default builders are collected in the `@angular-devkit/build-angular` package. The schemas configure options for the following builders:
- app-shell
- browser
- dev-server
- extract-i18n
- karma
- protractor
- server
- tslint

### Configuring builder targets
The `architect` section of *angular.json* contains a set of Architect targets. Many of the targets correspond to the CLI commands that run them. Some additional predefined targets can be run using the `ng run` command, and you can define your own targets.
Each target object specifies the builder for that target, which is the npm package for the tool that Architect runs. In addition, each target has an options section that configures default options for the target, and a `configuration` section that names and specifies alternative configuration for the target.
```json
"architect": {
  "build": { },
  "serve": { },
  "e2e" : { },
  "test": { },
  "lint": { },
  "extract-i18n": { },
  "server": { },
  "app-shell": { }
}
```
- **The architect/build** section configure defaults for option of the `ng build` command.
- **The architect/serve** section overrides build default and supplies additional serve defaults for the `ng serve` command.
- **The architect/e2e** section override build-option default for building end-to-end testing apps using the `ng e2e` command.
- **The architect/test** section override build option defaults for test builds and supplies additional test-running defaults for the `ng test` commands.
- **The architect/lint** section configures default for options of the `ng lint` command, which performs code analysis on project source files. The default linting tool for Angular is TSLint.
- **The architect/extract-i18n** section configures default for options of the `ng-xi18n` tool used by the `ng x1i8n` command, which extract marked message strings from source code and outputs translation files.
- **The architect/server** section configures defaults for creating a Universal app with server-side rendering, using the `ng run <project>:server` command.
- **The architect/app-shell** section configures defaults for creating an app shell for a progressive web app (PWA), using the `ng run <project>:app-shell` command.

In general, the options for which you can configure defaults correspond to the command options list in the [CLI reference page](https://angular.io/cli) for each command. Note that all options in the configuration file must use camelCase, rather than dash-case.

### Build target
The architect/build section configures default for options for the ng build command. It has the following top-level properties.

|Property| Description|
|--------|------------|
|builder| The npm package for the build tool used to create this target. The default builder for an application (ng build myApp) is @angular-devkit/build-angular:browser, which use the webpack package bundler. Note that a different builder is used for building a library (ng build myLib).|
|options| THis section contains default build target options, used when no named alternative configuration is specified.|
|configurations| This section defines and names alternative configuration for different intended destination. It contains a section for each named configuration, which set the default options for that intended environment.|

### Alternate build configurations
By default, a production configuration is defined, and the `ng build` command has `--prod` option that builds using this configuration. The production configuration sets defaults that optimize the app in a numbers of ways, such as bundling files, minimizing excess whitespace, removing comments and the dead code, and rewriting to use short, cryptic names("minification").
You can define and name additional alternate configuration (such as stage, for instance) appropriate to your development process. Some examples of different build configuration are stable, archive and next used by AIO itself, and the individual local-specific configurations required for building localized versions of an app.

### Additional build and test options
The configurable options for a default or targeted build generally correspond to the options available for the `ng build`, `ng serve` and `ng test` commands. For details of those options and their possible values.

Some additional options can only be set through the configuration file, either by direct editing or with the `ng config` command.

|Options Properties|Description|
|assets| An object containing paths to static assets to add to the global context of the project. The default paths point to the project's icon file and its *assets* folder.|
|styles| An array of styles files to add to the global context of the project. Angular CLI supports CSS imports and all major CSS preprocessors: sass/scss, less and stylus.|
|stylePreprocessorOptions| An object containing option-value pairs to pass to style preprocessors.|
|scripts| An object containing Javascript script files to add to the global context of the project. The scripts are loaded exactly as if you had  added them in a <script> tag inside *index.html*.|
|budgets| Default size-budget type and thresholds for all or parts of your app. You can configure the builder to report a warning or an when the output reaches or exceeds a threshold size.|
|fileReplacements| An object containing files and their compile-time replacements.|

### Complex configuration values
The options *assets*, *styles* and *scripts* can have either simple path string values, or object values with specific fields. The *sourceMap* and *optimization* options can be set to a simple Boolean value with a command flag, but can also be given a complex value using the configuration file.

#### Asset configuration
Each build target configuration can include an assets array that lists files or folders you want to copy as-is when building your project. By default, the *src/assets* folder and *src/favicon.ico* are copied over.
```json
"assets": [
  "src/assets",
  "src/favicon.ico"
]
```
To exclude an asset, you can remove it from the assets configuration,

You can further configure assets to be copied by specifying assets as objects, rather than as simple paths relative to the workspace root. A asset specification object can have the following fields.
- **glob**: A node-glob using input as base directory.
- **input**: A path relative to the workspace root.
- **output**: A path relative to outDir(default is dist/project-name). Because of the security implications, the CLI never writes files outside of the project output path.
- **ignore**: A list of globs to exclude.

For example, the default asset paths can be represented in more detail using the following objects.
```json
"assets": [
  { "glob": "**/*", "input": "src/assets/", "output": "/assets/" },
  { "glob": "favicon.ico", "input": "src/", "output": "/" }
]
```
You can use this extended configuration to copy assets from outside your project. For example, the following configuration copies assets from a node package.
```json
"assets": [
 { "glob": "**/*", "input": "./node_modules/some-package/images", "output": "/some-package/" },
]
```
