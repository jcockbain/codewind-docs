---
layout: guide
summary_title: "Codewind in Eclipse"
title: "Getting Started with Codewind in Eclipse"
categories: guides
description: "Take advantage of Codewind's tools to help build high quality cloud native applications regardless of which IDE or language you use."
permalink: codewind-eclipse-quick-guide.html
duration: 5 minutes
tags: Codewind, Eclipse, microservice
objectives: ["Install Eclipse and Codewind.", "Develop a simple microservice that uses Eclipse Codewind in Eclipse."]
icon: images/learn/icon_logoEclipse.svg
---

## Overview

Use Eclipse Codewind to create application projects from Application Stacks that your company builds. With Codewind, you can focus on your code and not on infrastructure and Kubernetes. Application deployments to Kubernetes occur through pipelines when developers commit their local code to the correct Git repos Kabanero is managing through webhooks.

Use Codewind to create projects based on different template types. These projects include IBM Cloud Starters, OpenShift Do (odo), and Appsody templates. Today, there are templates for IBM Cloud Starters, odo, Eclipse MicroProfile/Java EE, Springboot, Node.js, Node.js with Express, and Node.js with Loopback.

## Developing with Eclipse
You can use Codewind for Eclipse to develop and debug your containerized projects from within a local Eclipse IDE.

### Prerequisite
Before you can develop a microservice with Eclipse, you need to:

* [Install Docker](https://docs.docker.com/install/)
    * **Note:** Make sure to install or upgrade to minimum Docker version 19.03.
* [Install Eclipse](https://www.eclipse.org/downloads/packages/release/)
    * **Note:** Make sure to install or upgrade to minimum Eclipse version 2020-03.

### Installing Codewind for Eclipse
The Codewind installation pulls the following images that form the Codewind backend:

1. `eclipse/codewind-performance-amd64`
2. `eclipse/codewind-pfe-amd64`

The Codewind installation includes two parts:

1. The Eclipse plug-in installs when you install Codewind from the [Eclipse Marketplace](https://marketplace.eclipse.org/content/codewind). Or from the Eclipse IDE, you can go to **Help**>**Eclipse Marketplace** then search for Codewind. 
2. The Codewind backend containers install after you click **Install**. Clicking **Install** downloads the Codewind backend containers, ~1GB.

### Configuring Codewind to use application stacks
Configure Codewind to use Appsody templates so you can focus exclusively on your code. These templates include an Eclipse MicroProfile stack that you can use to follow this guide. Complete the following steps to select the Appsody templates:

1. Click the **Codewind** tab.
2. Expand **Codewind** by clicking the drop-down arrow.
3. Right-click **Local [Running]**.
4. Select **Manage Template Sources...**.
5. Select **Appsody Stacks - incubator**.
6. Click **OK**.

When you configure Codewind to use Appsody templates, continue to develop your microservice within Codewind.

If your organization uses customized application stacks and gives you a URL that points to an `index.json` file, you can add it to Codewind:

1. Return to  **Codewind** and right-click **Local [Running]**.
2. Select **Manage Template Sources...**.
3. Click **Add...** to add your URL.
4. Add your URL in the `URL:` box in the pop-up window and save your changes.

### Creating an Appsody project
Appsody helps you develop containerized applications and removes the burden of managing the full software development stack. If you want more context about Appsody, see the [Appsody welcome page](https://appsody.dev/docs).

1. Right-click **Local [Running]** under **Codewind** in the **Codewind** tab.
2. Select **+ Create New Project...**
    * **Note:** Make sure that Docker is running. Otherwise, you get an error.
3. Name your project **appsody-calculator**.
4. Under **Template**, select **Appsody Open Liberty default template**. 
    * If you don't see an Appsody template, select the **Manage Template Sources...** link in the window.
    * Select the **Appsody Stacks - incubator** checkbox.
    * Click **OK**.
    * The templates are refreshed, and the Appsody templates are available.
5. Click **Finish**.
    * To monitor your project's progress, right-click your project and select **Show Log Files**.
    * Select **Show All**. Then, a **Console** tab is displayed where you see your project's build logs.

Your project is displayed in the **Local [Running]** section where the project's progress is tracked.

Your project is complete when you see that your project is running and its build is successful.

### Accessing the application endpoint in a browser

1. Return to your project under the Codewind tab.
2. Right-click your project and select **Open Application**.

Your application is now opened in a browser, showing the welcome to your Appsody microservice page.

### Adding a REST service to your application

1. Go to your project's workspace under the Project Explorer tab.
2. Go to **Java Resources** then find `/src/main/java/dev.appsody.starter`.
3. Right-click **dev.appsody.starter** and select **New**>**Class**.
4. Create a Class file, name it **Calculator.java**, and select **Finish**. This file is your JAX-RS resource.
5. Before you input any code, make sure that the file is empty. 
6. Populate the file with the following code and then **save** the file:

```java
package dev.appsody.starter;

import javax.ws.rs.core.Application;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;

import javax.ws.rs.PathParam;

@Path("/calculator")
public class Calculator extends Application {

    @GET
    @Path("/aboutme")
    @Produces(MediaType.TEXT_PLAIN)
    public String aboutme() {
        return "You can add (+), subtract (-), and multiply (*) with this simple calculator.";
    }

    @GET
    @Path("/{op}/{a}/{b}")
    @Produces(MediaType.TEXT_PLAIN)
    public Response calculate(@PathParam("op") String op, @PathParam("a") String a, @PathParam("b") String b) {
        int numA = Integer.parseInt(a);
        int numB = Integer.parseInt(b);

        switch (op) {
            case "+":
                return Response.ok(a + "+" + b + "=" + (Integer.toString((numA + numB)))).build();

            case "-":
                return Response.ok(a + "-" + b + "=" + (Integer.toString((numA - numB)))).build();

            case "*":
                return Response.ok(a + "*" + b + "=" + (Integer.toString((numA * numB)))).build();

            default:
                return Response.ok("Invalid operation. Please Try again").build();
        }
    }
}
```
Any changes that you make to your code are automatically built and redeployed by Codewind, and you can view them in your browser.

### Working with the example calculator microservice
You now can work with the example calculator microservice.

* Use the **Exposed Application Port** number from the Application overview tab.
* Make sure to remove the `< >` symbol in the URL.
* `http://127.0.0.1:<port>/starter/calculator/aboutme`
* You see the following response:

```
You can add (+), subtract (-), and multiply (*) with this simple calculator.
```

You can try a few of the sample calculator functions:

* `http://127.0.0.1:<port>/starter/calculator/{op}/{a}/{b}`, where you can input one of the available operations `(+, _, *`, and an integer a, and an integer b.
* So for `http://127.0.0.1:<port>/starter/calculator/+/10/3` you see: `10+3=13`.

## What you have learned
In this quick guide, you have learned to:
1. Install Codewind on Eclipse
2. Develop your own microservice that uses Codewind

## Next Steps
See other quick guides to learn how to develop with Codewind:

* [Codewind in VS Code](codewind-vscode-quick-guide.html)