---
path: "./2019/11/23/Preparing-Jenkins-after-installation.md"
date: "2019-11-23"
title: "Preparing Jenkins after Installation"
description: "Easy to understand tutorial: Preparing Jenkins after installation"
lang: "en-us"
---

### Add Credentials ###
- Add dockerhub credentials _(Username does not mean email address)_
- Add sonarqube credentials

For each of the above do the following to add credentials  
  * Manage Jenkins -> Configure Credentials
  * Click _Credentials_ on the left panel
  * Hover over _(global)_ and click on the drop down suggestion arrow by the side
  * Click _Add credentials_

### Install JaCoCo Plugin ###
- Install the JaCoCo Plugin.
  * Manage Jenkins -> Manage Plugins
  * Check if its installed, if not go to available tab to search for JaCoCo
  * Tick JaCoCo checkbox and install it
This plugin allows you to capture code coverage report from JaCoCo. Jenkins will generate the trend report of coverage and some other statistics.

### Install Pipeline Utility Steps Plugin ###
- Install the Pipeline Utility Steps Plugin.
  * Manage Jenkins -> Manage Plugins
  * Check if its installed, if not go to available tab to search for the plugin
  * Tick the plugin's checkbox and install it
This will enable you do things like reading the maven pom:
```groovy
pipeline {
    agent any
    environment {
        IMAGE = readMavenPom().getArtifactId()
        VERSION = readMavenPom().getVersion()
    }
}
```

### Configure Webhooks ###
This is applicable if you have a Jenkins server (not localhost). In such case,
you configure Jenkins machine to communicate with your GitHub repository.
For that, we need to get the Hook URL of the Jenkins machine.

- Go to Manage Jenkins and select the Configure System view.
- Find the GitHub Plugin Configuration section and click on the Advanced button.
- Select the Specify another hook URL for GitHub configuration
- Copy URL in the text box field and unselect it.
- Click Save it will redirect to the Jenkins dashboard.
- Navigate to the GitHub tab on the browser and select your GitHub repository.
- Click on Settings. It will navigate to the repository settings.
- Click on the Webhooks section.  
- Click on Add Webhook button. Paste the Hook URL on the Payload URL field.
- Make sure the trigger webhook field has Just the push event option selected.
- Click Add webhook and it will add the webhook to your repository.
- Once you've added a webhook correctly, you can see the webhook with a green tick.

### Configure Email ###
- Configure Email Extension plugin
  * Check for Email Extension plugin by following this: Manage Jenkins -> Manage Plugins
  Email Extension should be under installed, if not check under available and install it:
  * Manage Jenkins -> Configure System
  * Look for section: _Extended Email Notification_
  * SMTP Server: smtp.gmail.com
  * Click Advanced
  * Check option: use SMTP Authentication and enter your username and password
  * Check option: use SSL
  * SMTP port: 465   
- Also Configure Email plugin via the section: _Email Notification_
- Configure Docker label
  * For both Declarative Pipeline (Docker) and Pipeline Model Definition
    - Set label to: docker
    - Set dockerhub credentials to earlier defined credentials

- To be able to send emails from any gmail account you entered above:
  * Browse to [the security section of your google account](https://myaccount.google.com/security)
  * Scroll to: Less secure apps access
  * Grant access to less secure apps

### References ###

- [DZone - Multibranch Pipeline in Jenkins](https://dzone.com/articles/implement-ci-for-multibranch-pipeline-in-jenkins)
