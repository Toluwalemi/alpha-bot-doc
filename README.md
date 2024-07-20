# HNG-DevOps-1(Alpha Bot) Documentation

## OVERVIEW

When it comes to software development a robust CI/CD (Continuous Integration/Continuous Deployment) system is a must and inexcusable.

One such CI/CD system you could have in place is this custom **Alpha bot**, a `Node.js` application. The bot automates building, deploying and merging pull requests (PRs)—every pull request automatically get its own testing environment, isolated from the main codebase.

It uses **Docker** for containerization in isolated environments, exposing deployed URL via **ngrok**, clean up of containers and resources and provides comprehensive feedback on the deployment via comments. It is repository agnostic.

This would mean new features and bug fixes are evaluated in a production-like setting before merging, which can be game-changing.

Such a setup offers an:

- **Effortless Testing of New Features**: Automatically deploy each new pull request into its Docker container, providing a pristine environment that mirrors production.
- **Enhanced Code Review Process**: Enable reviewers to interact with live, deployed versions of the code changes, facilitating thorough testing and validation.
- **Optimized Resource Management System**: Automate the cleanup of Docker containers upon pull request closure, ensuring optimal use of infrastructure without manual intervention.

# Project Team Contributors

| Name     | LinkedIn |
| -------- | -------- |
| John Doe | link.com |
| Jane Doe | link.com |

<a id="table-contents"></a>

## Table of Contents

- [Prerequisites](#prerequisites)
- [Architecture](#architecture)
- [Folder Structure](#folder-structure)
- [Getting Started](#getting-started)
  - [Section A](#section-a)
  - [Section B](#section-b)

## Prerequisites to Get Started

Before diving into the setup, let's ensure you have all the necessary tools and knowledge to make this journey smooth and successful. However, we are making a couple assumptions, that you:

1. Understand Docker and have used it for deployment purposes
2. Understand version control systems (VCS)
3. Can find your way around Javascript
4. Have your linux cloud server up and running

For this installation to go properly, you'll need to:

### Install Docker

For Ubuntu:

```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

For MacOS:

```
brew install --cask docker
```

For Windows:

Download and install Docker Desktop from [Docker's official website](https://docs.docker.com/desktop/install/windows-install/).

Testing Docker Installation:

Once you have Docker installed, verify the installation by running:

```
docker --version
docker run hello-world
```

If everything is set up correctly, Docker will pull the “hello-world” image and run it in a container, displaying a success message like this:

```
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub. (amd64)
 3. The Docker daemon created a new container from that image which runs the executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
$ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
https://hub.docker.com/

For more examples and ideas, visit:
https://docs.docker.com/get-started/

$ docker images hello-world
REPOSITORY    TAG       IMAGE ID       SIZE
hello-world   latest    d2c94e258dcb   13.26kB
```

### GitHub Account

Make sure you have access to the repository where the deployment system will be set up. This is essential for integrating the GitHub App and bot with your workflow.

To effectively set up and manage this automated deployment system, you should have a basic understanding of the following:

- JSON: For handling data structures and API responses.
- JavaScript: Useful for developing the GitHub bot and handling webhook events.
- Webhooks: Mechanisms for receiving real-time updates from GitHub.
- APIs: Interfacing with GitHub and other services programmatically.

### ngrok Installation

ngrok is a powerful tool that allows you to expose your local development environment to the internet. This is particularly useful for testing webhooks (which we will be doing at the end of this deployment).

First, visit the ngrok website and [download the version suitable for your operating system](https://ngrok.com/download) (if you are using other non-ubuntu operating systems). On ubuntu, you can simply run:

```
$ curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null && echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list && sudo apt update && sudo apt install ngrok
```

Once installed, connect your ngrok account to gain access to advanced features. To do so, you have to sign-up to ngrok, it takes only a couple of minutes. On sign-up, you will be directed to verify your email address.

![ngrok sign-up page](images/ngrok.png)

After verifying your email, follow a couple prompts to get to your dashboard. Click on the Your Authtoken button on the side panel:

![ngrok side panel](images/Screenshot%202024-07-20%20181055.png)

On the next screen, click on the Copy button as shown by the arrow:

![ngrok authtoken](images/Screenshot%202024-07-20%20181326.png)

This automatically copies your Authtoken. On your command line, run the following command in your terminal, replacing `<your_auth_token>` with the token from your ngrok dashboard:

```
$ ngrok authtoken <your_auth_token>
```

With these prerequisites in place, you're well-prepared to set up your automated deployment system.

<a id="architecture"></a>

## Architecture

![Bot Architechture](images/alpha-bot.png)
The application listens for pull request events (`opened`, `reopened`, `synchronize`, `closed`) and performs the following actions:

- **Opened/Reopened/Synchronize**: Triggers a deployment of the pull request code using Docker and adds a comment to the pull request.
- **Closed**: Removes the deployed Docker container and its associated resources, and adds a comment to the pull request

<a id="folder-structure"></a>

## Folder Structure

```
|--- services
|    |--- deploymentService.js
|    |--- repositoryService.js
|--- .gitignore
|--- index.js
|--- package.json
|--- README.md
```

- services/deploymentService.js: Handles Docker deployment operations
- services/repositoryService.js: Manages interactions with the GitHub API
- index.js: The main application file that sets up the server and handles incoming webhook events.

## Functionality / Features

- Automated Pull Requests Handling:
- Resource Management:
- Comprehensive Feedback Comments:
- Publicly Accessible Endpoints:
- Custom Domain name:
- Error Handling??

<a id="getting-started"></a>

## Getting Started

<a id="section-a"></a>

### Section A: Alpha-Bot Usage

Alpha-Bot is Repository agnostic, it works on any and every respository it is set up for as far as it revolves around containerization of the code.
A few preresquities before running.

- A Github account
- The intended respository to be automated. Docker File not present, Create [here](https://medium.com/@swalperen3008/what-is-dockerize-and-dockerize-your-project-a-step-by-step-guide-899c48a34df6#:~:text=When%20you%20%E2%80%9CDockerize%E2%80%9D%20an%20application,application%20as%20a%20Docker%20container.)

#### Step 1. Intergration with Intended Repository

- To install the Alpha-bot application, [Click here](https://github.com/apps/alphateam-hng-devops)
  ![Installation](images/realstepa)
- Next select the repository permissions it should have access to, it then reflects on the applications page
  ![permissions](images/realstepb)
  ![view](images/realstepc)

[Back to Table of Contents](#toc)

### Section B: Alpha-Bot Testing

### Section C: Alpha-Bot Local Development/SetUp

A local run through of developing the Alpha-Bot from scratch.
Prerequisites for the set up:

- A Github Account
- A Liunx Server preferably Ubuntu
- Basic Javascript Knowledge

#### Step 1. Create a Github App

- At the top right corner of the page, select the github profile photo, scroll down to **settings** and click
  ![settings](images/stepa)
- Scroll the next drop down and select **developer settings** then **Github Apps** and finally **New Github App**
- The Requirements form pop, fill as follow
  ![Requirements](stepd.png)
- For the webhook url,insert the url pointing to your ngrok URL or any public URL where the bot is running.
- Webhook secret,

#### Step 2. Set the permissions:

- Repository permissions: Read & write for Issues.
- Subscribe to events: Choose Pull requests.

#### Step 3. Installation of the GitHub App:

- Install the app on your desired repositories.
- Generate and Download Private Key:

Download the private key and save it in your project directory. Update the .env file with the path to this private key.

### Step 2. Clone Repository

On your local machine, clone the project

```bash
git clone https://github.com/your-username/github-bot.git
```

```
cd github-bot
```

### Step 3. Install Dependencies

Its a nodejs app, npm package manager

```shell
npm install
```

```
node- --version
```

### Step 4: Code Blocks Explanation

- `package.json` file manages the projects dependencies and scripts
- `index.js` sets up the Express server and defines the webhook endpoint

```js
import express from "express";
import bodyParser from "body-parser";
import "dotenv/config";
import { addPRComment, verifySignature } from "./services/repositoryService.js";
import {
  triggerDeployment,
  removeDeployedContainer,
} from "./services/deploymentService.js";
```

- express: Framework for building web servers.
- body-parser: Middleware to parse incoming request bodies.
- dotenv/config: Loads environment variables from a .env file.
- addPRComment, verifySignature: Functions from repositoryService.js.
- triggerDeployment, removeDeployedContainer: Functions from deploymentService.js.
  Server Set Up

```js
const app = express();
const port = process.env.PORT || 3003;
app.use(bodyParser.json());
```

- app: Creates an Express application.
- port: Port on which the server will listen.
- app.use(bodyParser.json()): Middleware to parse JSON request bodies.
  Webhook Endpoint

```js
app.post("/webhook", async (req, res) => {
  const payload = JSON.stringify(req.body);
  const signature = req.headers["x-hub-signature"];
  console.log(signature);

  if (!verifySignature(payload, signature)) {
    return res.status(400).send("Invalid signature");
  }

  const event = req.headers["x-github-event"];
  console.log(event);
  if (event === "pull_request") {
    const action = req.body.action;
    console.log(action);
    const prNumber = req.body.number;
    const branchName = req.body.pull_request.head.ref;
    const repoName = req.body.repository.full_name;
    const repoUrl = `https://github.com/${repoName}.git`;
    const repoBranchName = `${repoName.replace("/", "_")}_${branchName.replace(
      "/",
      "_"
    )}`;
    console.log(repoBranchName);
    const imageName = repoBranchName.toLowerCase();
    const containerName = `${imageName}_pr_${prNumber}`.toLowerCase();

    if (
      action === "opened" ||
      action === "reopened" ||
      action === "synchronize"
    ) {
      try {
        const preDeployMessage = "The PR is currently being deployed...";
        await addPRComment(repoName, prNumber, preDeployMessage);
        const postDeployMessage = await triggerDeployment(
          repoUrl,
          branchName,
          prNumber,
          imageName,
          containerName
        );
        await addPRComment(repoName, prNumber, postDeployMessage);
        console.log("Deployment completed and comment added!");
      } catch (error) {
        console.error("Error during deployment:", error);
      }
    } else if (action === "closed") {
      try {
        const message = await removeDeployedContainer(containerName, imageName);
        await addPRComment(repoName, prNumber, message);
        console.log("Container removed and comment added");
      } catch (error) {
        console.error("Error during deployment:", error);
      }
    }
  }

  res.status(200).send("Event received");
});
```

- app.post('/webhook', async (req, res)): Defines the webhook endpoint to handle incoming GitHub events.
- payload: The payload of the incoming webhook event.
- signature: The signature to verify the webhook event's - authenticity.
- event: The type of GitHub event (e.g., pull_request).
- action: The action performed on the pull request (e.g., opened, closed).
- prNumber: The pull request number.
- branchName: The name of the branch associated with the pull request.
  -repoName: The full name of the repository.
- repoUrl: The URL of the repository.
- repoBranchName: A unique name for the branch.
- imageName: A lowercase version of the branch name to use as the Docker image name.
- containerName: A name for the Docker container.
  `deploymentService.js` file contains function for handling Docker deployments

```js
import { spawn } from "child_process";
import { promises as fs } from "fs";
import ngrok from "ngrok";
```

- spawn: Used to run shell commands.
- fs: Provides file system operations.
- ngrok: Exposes local servers to the internet.

### Step 5. Environment Variables Set-up

```env
PORT=3003
WEBHOOK_SECRET=your_github_webhook_secret
APP_ID=your_github_app_id
PRIVATE_KEY_PATH=path_to_your_private_key.pem
INSTALLATION_ID=your_installation_id
NGROK_AUTH_TOKEN=your_ngrok_auth_token
```

- `PORT`: The port number on which the application will run
- `WEBHOOK_SECRET`: The secret token configured in GitHub webhook settings
- `APP_ID`: GitHub App ID.
- `PRIVATE_KEY_PATH`: Path to the private key file generated for your GitHub App.
- `INSTALLATION_ID`: Installation ID of your GitHub App.
- `NGROK_AUTH_TOKEN`: Your ngrok authentication token.

### Step 6. Running the Application

Start the application

```bash
npm start
```

```bash
npm run dev
```

### Step 7. Expose Local Server

```bash
ngrok http 3003
```

#### Section D: GitHub App Bot testing

- Navigate to the repository you want to make contributions and fork it.
- Clone the repository to your local machine.
- Create a new branch,make changes,commit the changes and puush the changes
- Create a Pull Request in the repository where the GitHub App is installed. The bot should automatically start the deployment process.

- Verify Deployment:

  - Check the PR thread for a comment from the bot with the deployment status and URL.
  - Access the deployment using the provided URL to verify that the container is running.

- Close the PR: Closing the PR should trigger the cleanup process, removing the Docker container and associated resources.

## Collaboration Guide

We welcome contributions from the community to improve **Alpha-Bot**! Here's how you can get involved:

- Reporting Issues
- Suggesting Features
- Contributing Code
  > 1.  Fork the repository
  >
  > 2.  Create a new branch,
  >
  > 3.  Make your changes
  >
  > 4.  Test thoroughly

## Maintenance

## Troubleshooting

## Licence
