# CI/CD: Day 1

## GIT/GITHUB

These are exercises that will be done during assessment week in order to familiarize devs with git. We will be using Git and Github for these exercises.

### Create a Repo

Go to your [GitHub account](https://github.com), login, click on repositories then click new. At repository name field add **_hello-aca-github_**. Under description just add **_This is my first experience with git under ACA program_**. Leave it as public, then check the add readme file option. Finally click on create repository.

### Clone

Click on “**_code”_** dropdown then clone (copy the link of) the remote repository to create a local repository on your dev machine.

Open your terminal and type `git clone <copied url>`

### Add

Open VSCode, create an **_index.html file_** within the cloned directory (project folder) and add basic structure that prints “**_i love aca_**.” Then create **JS** and **CSS** folders with appropriate **empty files(js and css files)** inside. Add the changes in the working directory to the staging area.

### Status

Back in your terminal, check the state of the working directory and the staging area of any pending changes.

### Commit

Commit changes with the appropriate messages, the message should briefly best describes the applied changes. Use present tense and imperative mood eg **_Modifies the calendar app header_**

### Push

Push the changes, this will take the committed in the local repository and uploads them to the remote repository.

### Create Branch

Create a new branch called **_toggle_button_feature_** in your local repository

### Checkout (repeat the previous steps to commit new code)

Navigate from branch **main** to branch **toggle_button_feature**. Add javascript and css code that creates a toggle button which when pressed it prints out “**_ACA just taught me about continuous integration and continuous deployment_**.” Repeat the steps Add, Status, Commit and Push but in the new branch (toggle_button_feature).

### Merge

On your remote repository in the github interface, create a **Pull** **Request** to merge your toggle\*button_feature branch to your main branch. Add this comment to the PR \*\*\*“This is my first PR in the ACA program”.\_\*\* Then complete the PR merge yourself.

### Hosting Server (Heroku)

Hosting server are facilities that holds built code (product) so it can be accessed by people on the internet. For this exercise we are going to use Heroku and in the future we will use other hosting platforms like Netlify, AWS, Azure etc

1. Create a Heroku Account ([https://www.heroku.com/](https://www.heroku.com/))
2. Create a new app
3. Add buildpack as a php project (This is because heroku does not support hosting of static websites - html, js, css- ). This is why you will need two other file in the root of your repo.
   1. Composer.json
   2. index.php
4. Copy the App name and the API key. You will need them to setup the build server

Note:

Buildpacks are scripts that are run when your app is deployed. They are used to install dependencies for your app and configure your hosting environment.

### Continuous Integration Server (Build Server)

A continuous integration server also commonly called a build server is a centralized, stable and reliable environment for building distributed dev projects and help ensure quality of code and project with automated tests, reporting, code integrity etc You will know more as you progress in the program. For this we are going to use Circle CI. The config file has been provided for you. To get it up and running follow the steps below

1. Create a CircleCI project and connect it with your Github repo.
2. Ensure the .circle directory is at the root of your project in your repository
3. Add **HEROKU_APP_NAME** and **HEROKU_API_KEY** to the project environment variables so that CircleCI can connect to the Heroku app (Hosting server)
4. Open the CircleCI config.yml file and you will see two jobs. The build job and the deploy job. The build job is to “compile” the code and ensure its not breaking. The deploy job is to push the build artifact to the hosting location to be ran.

Note:

On CircleCI dashboard click on the **Project Settings** for the Your App, and then click on **Environment Variables**.

On the **Environment Variables** page, create two variable named **HEROKU_APP_NAME** and **HEROKU_API_KEY** and give them their respective values as gotten from your Heroku dashboard.

To get your **HEROKU_API_KEY**, go to your **Heroku Dashboard**, click on **Account Settings**, then scroll down to the **API Key section** and click on **Reveal** to copy your **API key**.

**HEROKU_APP_NAME** is the name of your Heroku app.

Initial files and directory need at the root of the dev repo

1. .circleci/config.yml
2. composer.json
3. index.php

### config.yml

```
# Use the latest 2.1 version of CircleCI pipeline process engine.
# See: https://circleci.com/docs/2.0/configuration-reference
version: 2.1

orbs:
  heroku: circleci/heroku@1.2.6 # Invoke the Heroku orb

# Define a job to be invoked later in a workflow.
# See: https://circleci.com/docs/2.0/configuration-reference/#jobs
jobs:
  build:
    # Specify the execution environment. You can specify an image from Dockerhub or use one of our Convenience Images from CircleCI's Developer Hub.
    # See: https://circleci.com/docs/2.0/configuration-reference/#docker-machine-macos-windows-executor
    working_directory: ~/repo
    docker:
      - image: cimg/node:12.16
    # Add steps to the job
    # See: https://circleci.com/docs/2.0/configuration-reference/#steps
    steps:
      - checkout
      - run:
          name: Update NPM
          command: "sudo npm install -g npm"
# Invoke jobs via workflows
# See: https://circleci.com/docs/2.0/configuration-reference/#workflows
workflows:
  version: 2
  build-deploy:
    jobs:
      - build
      - heroku/deploy-via-git: # Use the pre-configured job, deploy-via-git
          requires:
            - build
          filters:
            branches:
              only:
                - main
```

### composer.json

```
{}

```

### index.php

```
<?php include_once("index.html"); ?>

```

Reference that might be useful:

1. [https://www.freecodecamp.org/news/an-introduction-to-git-for-absolute-beginners-86fa1d32ff71/](https://www.freecodecamp.org/news/an-introduction-to-git-for-absolute-beginners-86fa1d32ff71/)
2. [https://dev.to/mariehposa/how-to-set-up-continuous-integration-and-deployment-with-circleci-1pe9](https://dev.to/mariehposa/how-to-set-up-continuous-integration-and-deployment-with-circleci-1pe9)
