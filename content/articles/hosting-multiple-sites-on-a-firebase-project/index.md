---
title: "Hosting Multiple Sites on a Single Firebase Project"
description: "No need to create separate firebase projects for each related domain you own, simply Host them on a single project"
date: 2022-10-15T11:43:13+01:00
tags: ["firebase", "hosting"]
categories: ["Hosting", "Firebase"]
draft: false
author: "@Colman1000"
cover:
    image: "hosting-multiple-sites-on-a-firebase-project.png"
    alt: "Hosting Multiple Sites on a Single Firebase Project | Cover Image"
    caption: "Hosting Multiple Sites on a Single Firebase Project"
images: ['hosting-multiple-sites-on-a-firebase-project.png']
keywords:  ["firebase", "hosting"]
summary: "No need to create separate firebase projects for each related domain you own, simply Host them on a single project"
---

[Firebase](https://firebase.google.com/), an app development platform, backed by Google, that helps developers rapidly 
build mobile and web apps that scale.

When it comes to **hosting** your web applications, [Firebase Hosting](https://firebase.google.com/products/hosting) can host static
websites and dynamic sites built with React, Vue or any Single Page Application (SPA) with a single command, `firebase deploy --only hosting`.

This works great for many use-cases however, you may run into a situation where you'd want to host multiple websites on 
the same firebase project, for example, say you want your website to support multiple languages such as *French* and *German*
but still want to share the same *Firestore Database* and maybe you also enabled Firebase analytics and you want data for 
all three sites aggregated on a single dashboard, Using separate projects won't cut it... In comes our subject matter, 
**Hosting Multiple Sites on a Single Firebase Project**.

### Prerequisites

To be able to follow along with this article, you'd need the following;

* A Google Account
* A basic knowledge of HTML
* Know your way around a terminal
* Get a beverage, Tea or Coffee ☕

### Create a Firebase Project
Head on to the [Firebase Console](https://console.firebase.google.com) and create a new project 

{{< figure src="hosting-setup-1.png" title="Create a Firebase Project" align="center" >}}

Give the new project a descriptive name

{{< figure src="hosting-setup-2.png" title="Give the Project a Name" align="center" >}}

And follow the remaining prompts.

### Enable Firebase Hosting
For this to work, you would have to enable hosting on the firebase project. Select `Hosting` from the side and click on *Get Started*
{{< figure src="hosting-step-1.png" title="Get started with hosting" align="center" >}}

### Install the Firebase CLI

If you do not already have the *Firebase CLI* installed, install it by running, 
```shell
npm install -g firebase-tools
```

{{< figure src="hosting-step-2.png" title="Install Firebase Tools" align="center" >}}

### Initialize Your Project

Before you initialize your project, you need to sign in to firebase. Run 
```shell
firebase login
``` 
to start the sign-in process. This command opens a tab in your default browser, so you can log in...

*PS: Login to the same Google account we used to create the Project in [Step 1](/articles/hosting-multiple-sites-on-a-firebase-project/#create-a-firebase-project)*

{{< figure src="hosting-step-3.png" title="Initialization instructions" align="center" >}}

Initialize your firebase project with the command: 
```shell
firebase init
``` 
and follow the prompts

{{< figure src="hosting-cmd-1.png" title="Initialize Your Project - Command Line" align="center" >}}

### Deploy Your Project

Now we deploy the first site. Simply run 
```shell
firebase deploy --only hosting
```

**Congrats 🥳!!!** You have successfully deployed your first site.

### Setup Second Site

For our second site, we'd need to setup the site first on the firebase console...

Head on to the **Hosting** section, scroll down to the bottom and click on **Add another site**

{{< figure src="hosting-step-5.png" title="Setup Second Project" align="center" >}}

Enter a *unique name* for your project then select **Add site**.

*PS: you will need this **unique name** in the next step.*

{{< figure src="hosting-step-6.png" title="Add Site" align="center" >}}

### Setup Second Project Locally

Open the second project on your computer... This project can be as simple as only containing a single `html` file.

Run the following command to connect your project to the firebase project

```shell
firebase init
```

Follow the prompts,selecting the same project as you did in the earlier steps.

Next, run the following to connect the second site from the firebase console to the local project

```shell
firebase target:apply hosting <alias> <unique_name>
```

Where: 
* `alias` can be any user-friendly name to represent the project
* `unique_name` is the same as in the [Previous Step](/articles/hosting-multiple-sites-on-a-firebase-project/#setup-second-site) above.

### Deploy Second Site

Finally, to deploy to the second site, Run;

```shell
firebase deploy --only hosting:<alias>
```

`alias`is the same as the previous step

**Voila 🎉**

To host more sites, simply follow the steps to [Add Second Site](/articles/hosting-multiple-sites-on-a-firebase-project/#setup-second-site) 
again

----

Thank You 🥳.

**Suggestions and Corrections are very welcome, Please comment below**