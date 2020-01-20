---
title: Using API Doc for your Web API's
date: 2020-01-20T13:11:39+00:00
author: Richard Brynteson
layout: post
permalink: /2020/01/20/using-apidoc-for-webapi/
categories:
  - Microsoft Teams
  - Skype for Business
  - Development
---

As we all get ready to write more code in 2020 we really need to think about ways to automate the code documentation process.  Even if the project is just being used by yourself, getting into the habit of writing solid documentation is a plus for long term sanity.

## What Is Good API Documentation

As I've had to interact with more and more products outside of Skype for Business you will move from old COM based tools to REST based API's instead.  This is what the modern world is built upon.  If you are using a service like Microsoft Graph you may be familiar with [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer) but their real documentation for 1.0 endpoints would be found [here](https://docs.microsoft.com/en-us/graph/api/overview?view=graph-rest-1.0).

The documentation will contain a list of Endpoints followed by what exactly you can accomplish with that end point.  For me, good documentation is not just about having a huge technical document but also having a workable example you can interact with.  Example of how to utilize the endpoint and what the return message would be as well.  Additionally, I would contend good documentation will also tell you specifically what each input and output does.

## Time to Automate

In my hunt of the internet I found a [tool](https://apidocjs.com/) to automate the create of API Documentation.  However, in my endeavors of trying to make it work for me I found that there wasn't much documentation on how to make this function with .NET Web API (v1 or v2 - didn't matter).  It's common practice to use parameters in the URL path and not in the query string when using Web API standard.  So you might have an endpoint such as /Users/Variable1/Variable2 as opposed to /Users/?var1=blah&var2=blah.

Let's see how we could potentially automate your documentation.  How cool would it be if we could do this automatically:

<img src="https://theargylemvp.com/assets/images/1.20.2020.pic1.png" width="650" />

#### Step 1: Install the Tool

This is the easy step.  First, you need to make sure that you have npm installed on your workstation.  Browse to the repo path via the command line and run:

>> npm install apidoc -g

This will go through all the steps of installing the tool.  There are no specific setup options.  

Once the tool is installed then you need to get into the process of creating outline for documentation.  Now I know you are thinking, can't it just "magically" figure it out?  Nope, you still need to tell the app exactly what is going to be inputted.  So let's say we have the following GET endpoint.

```c#
[Route("Users/{APIKey}/{TenantID}")]
[HttpGet]
public IEnumerable<User> Users(string APIKey, string TenantID)
{
    //Create List of Multiple Contacts
    List<User> allUsers = new List<User>();

    if (ValidateAuth(APIKey, TenantID) == true)
    {
        // Some Code
        // Add Item to Array of allUsers
        User c = new User();
        c.something = something;
        allUsers.Add(c);
    }

    return allUsers;
}
```

As you can see above, we have a GET command that expected two parameters APIKey and TenantID that are passed in the URL.  Looking at the [apiDocs](https://apidocjs.com/) Getting Started guide it's not straight forward what to do next.  Their getting started guide they tell you to run the tool via the command line.  But you haven't defined any documentation yet so you will get nothing in return.  So our first step is to update our API with the documentation keywords.

```c#
[Route("Users/{APIKey}/{TenantID}")]
[HttpGet]
public IEnumerable<User> Users(string APIKey, string TenantID)
{
    /**
    * @api {get} /Users/:APIKey/:TenantID Get all users
    * @apiVersion 1.0.0
    * @apiName User
    * @apiGroup Testing
    * @apiDescription Return all users for a specific Tenant
    * 
    * @apiParam {String} APIKey Authorization value generated API Key
    * @apiParam {String} CustTenantIDomerID Multi-Tenant Customer ID
    * 
    * @apiSuccess {String} Name Display Name of User
    * @apiSuccess {String} SipAddress Sip Address of User
    * @apiSuccess {String} LineURI Phone number (and extension) assigned to user
    * @apiSuccess {String} WindowsEmailAddress E-Mail Address of user
    * @apiSuccess {String} UserPrincipalName UPN of User
    * @apiSuccess {String} O365TeamsUpgradeEffectiveMode Teams upgrade policy (used for Teams Routing)
    * @apiSuccess {String} O365VoicePolicy Teams voice policy (used for Teams Routing)
    * @apiSuccess {String} O365VoiceRoutingPolicy Teams routing policy (used for Teams Routing)
    * 
    * @apiSampleRequest /Users/:APIKey/:TenantID
    */

    //Create List of Multiple Contacts
    List<User> allUsers = new List<User>();

    if (ValidateAuth(APIKey, TenantID) == true)
    {
        // Some Code
        // Add Item to Array of allUsers
        User c = new User();
        c.something = something;
        allUsers.Add(c);
    }

    return allUsers;
}
```

So you can now see that we have added our Documentation Template language to the top of our Web API.  Let's go through each one so you understand what it does.

>> @api

This is the full endpoint of your API.  In my example it is a GET command followed by the URL of /Users and then the two parameters that are expected.  You will notice that the parameters start with a : and that is import for the sample request below.

>> @apiVersion

The version of the API.  As you update your API you can version them and then your end users can compare what changed from version 1 to 2 for example.

>> @apiName

Just the endpoint name.

>> @apiGroup

We can group our endpoints for searchability.

>> @apiDescription

As you can guess, the description.

>> @apiParam

Here you describe the parameter passed to your API

>> @apiSuccess

Describe what is returned

>> @apiSampleRequest

This is the cool one.  You can setup your API docs so users can live test against the API.  This is great for those who are new to API's in general.

We are now close to being able to run the tool.  However, we need to define a apidoc.json that goes at the root of our Web API project.  Here we will define some key items.

```json
{
  "name": "Web API",
  "description": "API Docs",
  "title": "API Docs",
  "url": "https://api.somewhere.com",
  "sampleUrl": "https://api.somewhere.com",
  "template": {
    "withCompare": true,
    "withGenerator": true
  }
}
```

Above, you can see we have defined the name, url, sample URLs and more.  Now that we have that all done, now we can run the apiDoc tool.  

>> apidoc -i myapp/ -o apidoc/ -t mytemplate/

In the above:

- -i is the path of where your application is.  
- -o is the output folder
- -t is where the template folder is

The template is completely optional.  But that is it.  The ApiDoc tool will parse all your .cs files, find the API Documentation tags and create your docs for you.

What is great is because you have defined both a sampleUrl in your .json document and specified the URL in the documention, this will create a tool where users can enter these values and test what the input and output against your API is.

<img src="https://theargylemvp.com/assets/images/1.20.2020.pic2.png" width="850" />

Hopefully this is helpful in your documentation of Web APIs.
