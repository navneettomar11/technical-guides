# Using Cloud Foundry Command Line Interface (cf CLI)

## Getting Started with the cf CLI

### Localize
The cf CLI translates terminal output into the language that you select. The default language is `en-US`. 
Use `cf config` to set the language. To set the language with `cf config`, use the syntax:
> $ cf config --locale YOUR_LANGUAGE

For example, to set the language to Portuguese and confirm the change by running `cf help`:
> $ cf config --locale pt-BR
> cf help

### Login
Use `cf login` to log in to PWS. The `cf login` command use the following syntax to specify a target API endpoint, an org(organization), and a space: 
> cf login [-a API_URL] [-u USERNAME] [-p PASSWORD] [-o ORG] [-s SPACE]

- `API_URL`: This is your API endpoint, `api.run.pivotal.io`.
- `USERANAME`: Your username
- `PASSWORD`: Your password. Use of the `-p` option is discouraged as it may record your password in yourv shell history.
- `ORG`: The org where you want to deploy your apps.
- `SPACE`: The space in the org where you want to deploy your apps.

The cf CLI prompts for credentials as needed. If you are a member of multiple orgs or spaces, `cf login` prompt your for which one to log into. Otherwise it targets your org and space automatically.
> $ cg login -a https://api.example.com -u username@example.com
> API endpoint: https://api.example.com
> Password >
> Authenticating...
> OK
> Select an org (or press enter to skip):
> (1) development
> (2) staging
> (3) production
> Space> 1
> Target space development

Alternatively, you can write a script to login in and set your target using the non-interactive `cf api`, `cf auth` and `cf target` commands.

Upon successful login, the cf CLI saves a `config.json` file containing your API endpoint, org, space values, and access token. If you change these settings, the `config.json` file is updated accordingly.

By default, `config.json` is located in your `~/.cf` directory. The `CF_HOME` environment variable allows you to locate the `config.json` file wherever you like.

### Users and Roles
The cf CLI includes commands that list users and assign roles in orgs and spaces.
#### Commands for Listing users
These command takes an org or spaces an argument:
- cf org-users
- cf space-users
For example, to list the users who are members of an org:
> cf org-users example-org
>Getting users in org example-org as username@example.com...
>
> ORG MANAGER
>   username@example.com
> BILLING MANAGER
>   huey@example.com
>   dewey@example.com
> ORG AUDITOR
>   louie@example.com

#### Commands for Managing Roles
These commands require PWS admin permissions and take username, org or space, and roles as arguments:
- cf set-org-role
- cf unset-org-role
- cf set-space-role
- cf unset-space-role

Availabel roles are `OrgManager`, `BillingManager`. `OrgAuditor`, `SpaceManager`, `SpaceDeveloper`, and `SpaceAuditor`. For example, to grant the OrgManager role to a user with an org:
> $ cf set-org-role huey@example.com example-org OrgManager
> Assigning role OrgManager to user huey@example.com in example-org as username@example-org.com...
> OK


---
Note: If your are not a PWS admin, you see this messages when you try to run these commands:
`error code: 10003, message: You are not authorized to perform the requested action`
---

#### Indentical Username in Multiple Origins
If a username corresponds to multiple accounts from different user stores, such as both the internal UAA store and an external SAML or LDAP store, the `set` and `unset` role commands above return an error:
`The user exists in multiple origins. Specify an origin for the requested user from: 'uaaa', 'other'`

### Push
The `cf push` commands pushes a new app or syncs changes to an existing app.
If you do not provide a hostname (also known as subdomain), `cf push` routes your app to a URL of the form `APPNAME.DOMAIN` based on the name og your app and your default domain.
The `cf push` command supports many options that determine how and where the app instances are deployed. 

#### Always provide an App Name to cf push
`cf push` requires an app name, which you provide either in a manifest or at the command line.
`cf push` locates the `manifest.yml` in the current working directory by default, or in the path provided by the `-f` option.

If you do not use a manifest, the minimal `cf push` command look like this:
> $ cf push my-app


---
**Note**: When you provide an app name at the command line, `cf push` uses that app name whether or not there is different app name in the manifest. If the manifest describe multiple apps, you can push a single app by providing its name at command line; the cf CLI does not push the others.
---

#### How cf push Finds the App
By default `cf push` recursively pushes the contents of the current working directory. Alternatively, you can provide a path using either a manifest or a command line option.
- If the path is to a directory, `cf push` recursively pushes the contents of that directory instead of the current working directory.
- If the path is to a file, `cf push` pushes only that file.
---
Note: If you want to push more than single file, but not the entire content of a directory, consider using a `.cfignore` file to tell `cf push` what to exclude.
---

### User-Provided Service Instances
To create or update a user-provided service instance, you need to supply basic parameters. For example a database service might require a username, password, host, port, and database name.

The cf CLI has three ways of supplying these parameters to create or update an instance of a service: interactively, non-interactively, and in conjunction with third-part log management software as described [RFC 6587](http://tools.ietf.org/html/rfc6587). When used  with third-party logging, the cf CLI sends data formatted according to [RFC5424](http://tools.ietf.org/html/rfc5424)

You create a service instance with `cf cups` and update one with `cf uups` as described below.

#### The cf create-user-provided-service(cups) command
Use cf create-user-provided-service(alias cf cups) create a new service instance.
**To supply service instance parameters interactively**: Specify parameters in a comma-separated list after the `-p` flag. This example command-line session creates a service instance for a database service.
> cf cups sql-service-instance -p "host, port, dbname, username, password"
>host> mysql.example.com
>port> 1433
>dbname> mysqldb
>username> admin
>password>Pa55w0rd
>Creating user provided service sql-service-instance in org example-org / space development as username@example.com
>OK

**To supply service instance parameters to `cf cups` non-interactively**: Pass parameters and their values in as a JSON hash, bound by single quotes, after the `-p` tag. This example is a non interactive version of the `cf cups` session above.
>$ cf cups sql-service-instance -p '{"host":"mysql.example.com", "port":"1433", "dbname":"mysqldb", "username":"admin","password":"pa55woRD"}
>Creating user provided service sql-service-instance in org example-org / space development as username@example.com...
>OK

**To create a service instance that sends data to a third-party**: Use the `-l` option followed by the external destination URL. This example creates a service instance that sends log information to the syslog drain URL of third-part log management service.
>$ cf cups mylog -l syslog://logs4.example.com:25258
>Creating user provided service mylog in org example-org / space development as username@example.com...
>OK

After you create a user-provided service instance you bind it to an app with `cf bind-service`, unbind it with `cf unbind-service` rename it with `cf rename-service` and delete it with `cf delete-service`.

#### The cf update-user-provided service (uups) command
Use  cf update-user-provided-service(alias uups) to update one or more of the parameters for an existing user-provided service instance. The `cf uups` command use the same syntax as `cf cups` above to set parameter values. The `cf uups` command does not update any parameter values that you do not supply.

### cf CLI Return Codes
The cf CLI uses exit codes, which help with scripting and confirming that a command has run successfully. FOr example, after you run a cf CLI command, you can retrieve its return code by running `echo $?`(on Windows, `echo %ERRORLEVEL%`). If the return code is `0`, the command was successful.

### The cf help Command
The cf help command list the cf CLI commands and a brief description of each. Passing the `-h` flag to any command lists detailed help, including any alias. For example, to see detailed help for `cf delete`, run:
>$ cf delete -h
>NAME:
>   delete - Delete an app
>USAGE:
>   cf delete APP_NAME [-f -r]
>ALIAS:
>   d



