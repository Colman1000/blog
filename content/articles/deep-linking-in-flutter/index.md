---
title: "Deep Linking in Flutter"
description: "Link your Web and Mobile Apps, so they behave predictably regardless of platform"
date: 2022-09-02T14:33:02+01:00
tags: ["flutter", "deep linking", "universal links", "url schemes", "android","ios"]
categories: ["Flutter"]
draft: false
author: "@Colman1000"
cover:
    image: "deep-linking-in-flutter-cover.png"
    alt: "Deep Linking | Cover Image"
    caption: "Deep Linking in Flutter"
images: ['deep-linking-in-flutter-cover.png', 'shot-1.png', 'shot-2.png', 'shot-3.png', 'shot-4.png', 'shot-5.png']
keywords: ["flutter", "deep linking", "universal links", "url schemes", "android","ios"]
summary: "Link your Web and Mobile Apps, so they behave predictably regardless of platform"
---

Web apps revolutionized the way data is shared. Just with a simple Url string such
as `https://en.wikipedia.org/wiki/Deep_linking`,
you can save your users the hassle of locating that specific page themselves on the World Wide Web – saving their time
and significantly improving the user experience.

Deep links takes this same idea and ports it over to mobile devices; Tap on a url to open a specific page in your app
[ 😍 ], instead of having to download a complete Mobile Application then navigating your way around to get to the
desired page.

Flutter supports deep linking for iOS, Android and Web. Say you develop and application with flutter targeting all three
platforms and your users tap on your deep linked url, your app will open to the specified screen if they have your app
installed or simply open your web app, to the specified page. Awesome!.

*By the way, if you prefer a behaviour where in the case your users do not have your app installed when they tap on the
link, the appropriate App Store is opened, so they can download, install your app and then open the desired page, check
out how to accomplish that with [Firebase Dynamic Links](https://firebase.google.com/docs/dynamic-links)*

### Setup

Setting up Deep Links in flutter is quite straight forward, There are no external dependencies needed, simply use
[Named Routes](https://docs.flutter.dev/development/ui/navigation#using-named-routes) for your navigation,
[GetX Route Management](https://pub.dev/packages/get#route-management) or use any other package that uses
*[Navigator 2.0](https://blog.codemagic.io/flutter-navigator2/)* such as [Go Router](https://pub.dev/packages/go_router)
.

By default, web apps read the deep link path from the url fragment using the pattern:`/#/path/to/app/screen`, but you
can change this behaviour to the regular path without the leading `/#` by configuring the
[URL strategy](https://docs.flutter.dev/development/ui/navigation/url-strategies) for your app.

After setting up everything mentioned above, we'll need to configure both Android and iOS to be aware of the domain to
use.

### Android

Make the following changes to your **AndroidManifest.xml** file in `root/android/app/src/main/AndroidManifest.xml`.

* Add the following metadata tag and **intent** inside the `<activity>` tag with the name `.MainActivity`;

```xml

<!-- Deep linking -->
<meta-data android:name="flutter_deeplinking_enabled" android:value="true"/>

<intent-filter android:autoVerify="true">
<action android:name="android.intent.action.VIEW"/>
<category android:name="android.intent.category.DEFAULT"/>
<category android:name="android.intent.category.BROWSABLE"/>
<data android:scheme="http" android:host="ENTER-YOUR-DOMAIN-HERE"/>
<data android:scheme="https"/>
</intent-filter>

```

**NB:** Replace the `ENTER-YOUR-DOMAIN-HERE` entry above with the domain you want to use, such
as [colman.tk](https://colman.tk).

Now when a user with your app installed visits a link like `https://colman.tk/works`, your app open to the screen mapped
to `/works` path.

### iOS

To support universal links in your app, you need to take the following steps:

* Add an entitlement that specifies the domains your app supports:

Open the `Associated Domains` section in the `Capabilities` tab and add an entry for each domain that your app supports,
prefixed with **applinks:**, for example **applinks:www.colman.tk**.

{{< figure src="shot-1.png" title="Go to Signing & Capabilities" align="center" >}}

{{< figure src="shot-2.png" title="Tap Add" align="center" >}}

{{< figure src="shot-3.png" title="Select Associated Domains" align="center" >}}

{{< figure src="shot-4.png" title="Tap Add" align="center" >}}

{{< figure src="shot-5.png" title="Add Domains you support" align="center" >}}

Next,

* Create an `Apple App Site Association` file

> You need to supply a separate apple-app-site-association file for each domain with unique content that your app
> supports. For example, apple.com and developer.apple.com need separate apple-app-site-association files, because these
> domains serve different content. In contrast, apple.com and www.apple.com don’t need separate site association
> files—because both domains serve the same content—but both domains must make the file available. For apps that run in
> iOS 9.3.1 and later, the uncompressed size of the apple-app-site-association file must be no greater than 128 KB,
> regardless of whether the file is signed.

Create a file called *apple-app-site-association* with the following contents

```json
{
  "applinks": {
    "apps": [],
    "details": [
      {
        "appID": "TEAM-ID.BUNDLE-ID",
        "paths": [
          "*"
        ]
      }
    ]
  }
}
```

**NOTE: Don’t append .json to the apple-app-site-association filename.**

**NB:** Replace the `TEAM-ID` and `BUNDLE-ID` entry above with the appropriate TEAM-ID and APP-ID for your app. For our
example, the `appID` entry would be `M7Y84F****.com.colman.tk`

If You're not sure of your `TEAM-ID`, open
[https://developer.apple.com/account/#!/membership/](https://developer.apple.com/account/#!/membership/),
then scroll down to `TEAM-ID`.

For more info about customizing the pages you want to allow Deep Links to handle, see  
[the docs](https://developer.apple.com/library/archive/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW1)

Finally,

* Upload the `apple-app-site-association` file to your server

You need to upload the *apple-app-site-association* file to your server and place it either directly in the root
folder, such that it is accessible without redirects as `<DOMAIN>/apple-app-site-association`; for example
`https://colman.tk/apple-app-site-association`**OR** you can upload the file to the `.well-known` 
folder such as `<DOMAIN>/.well-known/apple-app-site-association`.

### Testing Deep Links

Testing is quite easy, simply tap on a link that conforms to your deep link configuration and watch the magic happen.

You can also execute the following snippets;

##### Android

```markdown
adb shell am start -a android.intent.action.VIEW \
    -c android.intent.category.BROWSABLE \
    -d "https://ENTER-YOUR-DOMAIN-HERE/path/to/page"
```
For more details, see the [Verify Android App Links](https://developer.android.com/training/app-links/verify-site-associations)
documentation in the Android docs.


##### iOS

```markdown
xcrun simctl openurl booted https://ENTER-YOUR-DOMAIN-HERE/path/to/page 
```

Learn more [here](https://docs.flutter.dev/development/ui/navigation/deep-linking) 