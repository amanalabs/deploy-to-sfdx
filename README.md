# SFDX Deployer

## Purpose

You have a dev hub, and an sfdx repo.  You'd like to let people spin up scratch orgs based on the repo, and these people might not feel like using a terminal, cloning the repo, loggin in to a dev hub, and executing sfdx commands.
* because they might not be developers (think admins, or even end users in a training scenario)
* because they might not be Salesforce developers (say you built an app and give your designer/CSS person github access to "make it cool")
* because you might have dev hub access and you don't want to give it to them
* because you want to let people test the app quickly
* because (like me) you're using it for workshops and demos
---

## Connected app setup
Create a connected app for JWT auth, with certificates, per the SFDX setup guide.


https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_auth_jwt_flow.htm


---

## Environment variables

### Required
* `HUB_USERNAME` the username from your dev hub
* `CONSUMERKEY` from your connected app

### Cloud only
* `CLOUDAMQP_URL` starts with amqp://, generated by installing the amqp add-on

### Plus one of the following, depending on where you're running
* `JWTKEY` use only in heroku cloud.  Cut/paste your server.key, including the ---- lines
* `LOCAL_ONLY_KEY_PATH` don't use this in the cloud.  Put it in your local .env file, and it needs to be an absolute path

### optional
* `UA_ID` for google analytics measurement protocol

Here's a heroku button so you can have your own instance of the Deployer

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https%3A%2F%2Fgithub.com%2Fmshanemc%2Fdeploy-to-sfdx)

---

## Local Setup (Mac...others, who knows?)

You'll need to have a local filesystem structure that kinda replicates what we're doing on the heroku dyno.
```
mkdir tmp
```

NOTE: This will overwrite your dev hub default.  If you go back to doing other sfdx local work, be sure to re-auth to your hub.

Then put your JWT server.key file from the connected app in the /app/tmp folder.  (In heroku cloud, this'll load from the environment variable **JWTKEY** but heroku local doesn't load multiline variables easily, so that's why the manual placement of the key file is needed).

You'll also need rabbitmq running locally (handled on heroku via cloudamqp)

https://www.rabbitmq.com/install-homebrew.html

which you'll start up with
`/usr/local/sbin/rabbitmq-server` (accept any popups to allow comms)

then start this app with
`heroku local`

---
## Debugging on [non-local] Heroku
if **hosted-scratch-qa** is the name of your app

`heroku logs --tail -a hosted-scratch-qa` will give you the logs.  This app uses console.log pretty heavily.

`heroku ps:exec -a hosted-scratch-qa --dyno=worker.1` lets you ssh into the dyno of your choice and take a look around, clean stuff up, etc.




---

## Example Repos with orgInit.sh scripts

https://github.com/mshanemc/DF17integrationWorkshops [![Deploy](https://raw.githubusercontent.com/mshanemc/deploy-to-sfdx/master/assets/sfdx_it_now.png)](https://hosted-scratch.herokuapp.com/launch?template=https://github.com/mshanemc/DF17integrationWorkshops)


https://github.com/mshanemc/community-apps-workshop-df17

https://github.com/mshanemc/process-automation-workshop-df17

https://github.com/mshanemc/df17-community-content-workshop

https://github.com/mshanemc/df17AppBuilding