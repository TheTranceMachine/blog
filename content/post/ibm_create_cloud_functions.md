+++
title = 'Create IBM Cloud Functions'
date = 2023-11-08T09:37:42-05:00
draft = false
summary = 'Tutorial on how to create and deploy IBM Cloud function actions'
+++

# Create and deploy IBM Cloud function actions

> Important!
> 
> IBM Cloud® Functions is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](https://cloud.ibm.com/docs/openwhisk?topic=openwhisk-dep-overview).
>
> If you’re an existing IBM Cloud® Functions user, you can continue to use the service until it is no longer supported in October 2024.
>
> Clients are encouraged to migrate their Functions actions and triggers to IBM Cloud® Code Engine. Alternatively, clients might decide to host their code on VPC virtual instances or in IBM Cloud Kubernetes or OpenShift for IBM Cloud® clusters.
>
> See the following topics for documentation about migrating from IBM Cloud® Functions to Code Engine.
>
> [Migrating Cloud Functions Code to Code Engine](https://www.ibm.com/blog/migrating-cloud-functions-code-to-code-engine)
> 
> [Migrating IBM Cloud Functions to Code Engine](https://cloud.ibm.com/docs/codeengine?topic=codeengine-fun-migrate)

>> If the above didn't discourage you and you have a valid reason to create your cloud functions outside of Code Engine then please continue reading.

>> Prerequisites:
>> 1. IBM Cloud account (pay-as-you go will do)
>> 2. Working knowledge of the terminal


1. Initialize new node.js project
```
npm init
```
2. Edit `package.json` and add the following:
```
{
  "name": "<function name>",
  "version": "1.0.0",
  "main": "main.js",
  "dependencies": {
    "axios": "^1.6.0"
  }
}
```
3. Run `npm install` in the root of the project. This will install `axios` dependency.
4. Create `main.js` file in the root of the project and add the following example code:
```
function main(params) {
      var msg = 'You did not tell me who you are.';
      if (params.name) {
          msg = `Hello, ${params.name}!`
       } else {
          msg = `Hello, Functions on CodeEngine!`
      }
      return {
          headers: { 'Content-Type': 'text/html; charset=utf-8' },
          body: `<html><body><h3>${msg}</h3></body></html>`
       }
  }
  
  module.exports.main = main;
```
5. Create `manifest.yml` file in the root of the project and the following code:
```
packages:
    main-package:
      version: 1.0
      license: Apache-2.0
      actions:
        main:
          function:main.js
```
6. Add the following code to `package.json` (It will be responsible for deploying the function to your IBM Cloud Functions instance).
```
...
"scripts": {
    "deploy": "ibmcloud fn deploy --manifest manifest.yml"
  }
```
> You can now use `npm run deploy` to deploy your function but it won't work as you require to be logged in to IBM Cloud instance in your terminal and point to the right Namespace of IBM Cloud Functions. Let's create both in the next steps.


7. Log in to your IBM Cloud instance in your terminal. You can find the command using The IBM Cloud UI and clicking the top right corner user icon, choosing the "Log in to CLI and API" option from the dropdown, and copying your login command.

![Log_into_IBM_Cloud1.png](../../images/Log_into_IBM_Cloud1.png)

![Log_into_IBM_Cloud2.png](../../images/Log_into_IBM_Cloud2.png)


8. Select the namespace in your terminal. You can find the command by clicking through the menus on the left sidebar from The IBM Cloud UI.

![Functions_Namespace_command1.png](../../images/Functions_Namespace_command1.png)

![Functions_Namespace_command2.png](../../images/Functions_Namespace_command2.png)

Follow the steps from the screen.
![Functions_Namespace_command3.png](../../images/Functions_Namespace_command3.png)

9. Now you can deploy your function by running `npm run deploy` from the root of your project.

10.  If the terminal response is `Success: Deployment completed successfully.` then go to IBM Cloud Function UI and click Actions from the left sidebar to see successfully deployed functions.

> I recommend following the documentation at [openwhisk-wskdeploy](https://github.com/apache/openwhisk-wskdeploy) to find out more about how to configure your manifest.yml.
