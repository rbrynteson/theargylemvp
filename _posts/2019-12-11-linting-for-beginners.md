---
title: Linting For Beginners
date: 2019-12-11T20:54:39+00:00
author: Richard Brynteson
layout: post
permalink: /2019/12/11/linting-for-beginners/
categories:
  - Microsoft Teams
  - Development
---

In today's world we are sharing and working on more code together than ever before. Even in small teams within a company or maybe even in a collective group online you might be sharing code development opportunities. This code is most likely being stored in GitHub or another git repository. The problem is when you have multiple people working on code it starts looking different depending on who is writing the code. This is where linting comes in.

## What is to lint?

Linting is the process of running a program that will analyse code for potential errors.  You can read all about it on the [Wikipedia Page](https://en.wikipedia.org/wiki/Lint_%28software%29).  Essentially, it is a process to make sure that the code is all matching.  So here we are going to configure a lint process on an existing project.

## Install and Configure Lint

So the first step we need to do is install lint to your workstation.  You can everything about [eslint](https://eslint.org/docs/user-guide/getting-started) on its site.  In order to install lint you will need to make sure you have npm installed on your workstation first.

Once npm is installed, open up a command prompt in the directory for your project and run:

>> npm install eslint --save-dev

<img src="https://theargylemvp.com/assets/images/12-11-Image1.png" />

After the install, it is very possible that Visual Studio Code will warn you that you have too many changes and you should ignore the node_modules directory.  This is a good idea.

<img src="https://theargylemvp.com/assets/images/12-11-Image2.png" />

Also, you may notice that this project does not have a packages.json file.  This will cause some issues, so we need to go back and create one by running:

>> npm init

And go through the prompts.  We can run the npm install command again and now it will run without any errors.  

<img src="https://theargylemvp.com/assets/images/12-11-Image3.png" />

Now that we have installed lint to our projects, we can configure our lint environment

>> npx eslint --init

When you run the above command, it will start walking you through the configuration options.  I am going to use the following options:

<img src="https://theargylemvp.com/assets/images/12-11-Image4.png" />

Then you can customize the newly created lint configuration file.  

<img src="https://theargylemvp.com/assets/images/12-11-Image5.png" />

From this point, you can run something like the following to test your JS file:

>> eslint "C:\folder\location\file.js"

This will show you all of the errors.  

<img src="https://theargylemvp.com/assets/images/12-11-Image6.png" />

You can add a --fix to the end of the above command it eslint will fix all of the errors it can.  However, it is even better to use eslint in Visual Studio Code.  So you can add eslint as an extension to Visual Studio Code:

<img src="https://theargylemvp.com/assets/images/12-11-Image7.png" />

Once you have done that all of the code will light up in red that needs to be resolved.  

<img src="https://theargylemvp.com/assets/images/12-11-Image8.png" />

Here, you can see there are a lot of problems to fix.

## Fix All At Once?

So one of the great things about eslint is we can prevent people from uploading new code that has not been validated.  Obviously this is the best solution because it would prevent people from adding code to your project that does not follow all of your rules.

To accomplish this we need to use something like githooks to force rules to happen on events.  The easy solution is to use [Husky](https://github.com/typicode/husky) for this.  So the first step is we need to install Husky.

>> npm install husky --save-dev

So now that we have, we want to run the lint process each time we push staged files.  To accomplish that, we need to use [Lint Staged](https://github.com/okonet/lint-staged)

>> npm install --save-dev lint-staged@beta

<img src="https://theargylemvp.com/assets/images/12-11-Image9.png" />

The last step is to update our packages.json to create a husky rule to run lint-staged on a commit.  To do that we add this:

<img src="https://theargylemvp.com/assets/images/12-11-Image10.png" />

So now if someone tries to publish code to your project with an error, it will prevent you from publishing until you fix it.

<img src="https://theargylemvp.com/assets/images/12-11-Image11.png" />

And that is it!  Now you have a shared repo that no one can publish code that does not follow the rules you setup.
