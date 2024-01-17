# API Developer guide

  

## Introduction

  

The goal of this document is to describe the back-end of the Area application, for developers.
With this guide, you will quickly be able to code and add your own actions and reactions!

  

## How to add a service

### Migration and model

Firstly you need to change the migration and refresh it. When you clone the project, the service migration has token for gmail, spotify, discord, chatgpt and slack.
You need to change the migration to accept the service access_token that you want to add.

Once this is done, you need to do the same with the Service model.
  

### Service controller

Now, in the serviceController search for the addService function. This function is made to change the acceessToken easily, it has a switch_case to check which token it is trying to change and if it is available in the Service table.
Simply add a case with your service name, and save the token in the database

    service.service_token = token

One last step, if you wish to disconnect and delete the token, you need to do the same in deleteServices.
Same switch case!


## How to add an automation

### How is this working

We are using Adonis scheduler to call a Cron function every 1 minute. You can change the timer in the file CronEveryMinute in Tasks folder.
The Cron function called is a parser. It will get very automations from every user, then parse the details.

It will check what's the type of service, if the access_token is defined, then call the handle of the service.

It will call all actions and reactions until the end of the automation.steps, which is the details of every action.

Then in this handle it will parse what action is selected, and call the right function. Then, after the action is successful, it calls back to the automation parser without the last action called, so it's recursive but has an end.
  

### AutomationService

Now that you understand how is this working, let's add your action.

First change the AutomationService file.
In the handleAutomation there is the same kind of switch case set up than in services, you simply need to check for the type of action, which is your service name. Create a handleServiceAutomation(automation, accessToken) function to call the handle of your service.

To verify if token is existent, change the switch case in getAcessTokenForService function. Same thing, add your service and return the token.

Create a file for your services action in the same folder, create a first action with this definition:

    export  default  async  function  handleSericeName(automation) {

 Import this function in your automationService and call it in the handleServiceNameAutomation.
 Now, when an action has your service as type, it will call the handle in your service file, and giving the details as arguments.
 
### YourServiceService

Once you are in your handle, do a switch case to call the right action.

The details of your action is in automation.steps[0]
Then use .action to get the action name, .details for the details > .detail1 .detail2 .detail3

Do what you need to do with your action, use the accesstoken whenever needed, you are close to have finished!

Once you called your action, you need to continue the chain.

For that, check if the number of steps is superior to 1. If it is, shift the automation.steps json and call back handleAutomation with the new automation you created as parameter.

    const  numberOfSteps = Object.keys(automation.steps).length
    automation.steps.shift()
    if (numberOfSteps > 1) {
	    handleAutomation(automation);
    }

That's it! Congrats for your first action!

### Add a trigger

If you wish to add a trigger instead, follow the same steps, but in your trigger function return a boolean.
True if the trigger is detected, false on default.

Then, check that value and go to the next step only when it's true.

    handleAutomation(automation);
