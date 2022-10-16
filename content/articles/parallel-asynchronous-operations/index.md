---
title: "Parallel Asynchronous Operations"
description: "Speed up your asynchronous operations by running them all at once"
date: 2022-10-09T11:43:13+01:00
tags: ["dart","javascript","async","shorts"]
categories: ["Shorts", "Dart"]
draft: false
author: "@Colman1000"
cover:
    image: "parallel-asynchronous-operations.png"
    alt: "Parallel Asynchronous Operations | Cover Image"
    caption: "Parallel Asynchronous Operations"
images: ['parallel-asynchronous-operations.png', 'parallel-async-ops.png']
keywords: ["dart","javascript","asynchronous operations","shorts"]
summary: "Speed up your asynchronous operations by running them all at once"
---

Asynchronous operations allow your programs to do 'other stuff' while waiting for another operation to finish.
This is an important feature in modern programming languages because some operations just take time to complete... For
example; `Fetching data over a network`. The time it would take your app to fetch data over the internet varies due to
many factors such as **Network Speed**, **User's Device Memory/Processing Power** etc... Hence, it would not be wise to
`wait` for data to be fetched before doing other things that are **not dependent** on the data being retrieved.

> NOTE: We will be using the `dart` programming language in the following examples but the concept should be the same
> for languages that support `asynchronous programming` such as *JavaScript*.
>
> Secondly, Though `Streams` are also Asynchronous Operations in dart, we'll be focusing on `Futures` as they
> illustrate our point easily.

Suppose we want to perform the following operations;

* Get how many people are currently in space now from this endpoint: `http://api.open-notify.org/astros.json`
* Parse the json response to dart objects
* Make another request to our fictitious backend to grab `Space Craft Images`
* Parse the spacecraft image request response to dart objects
* Display result in our UI.

Assume we have the following function for making requests to any API and getting back a `ServerResponse` object which
contains the `response code` and `json string` as response from calling the api endpoint. 

```dart
ServerResponse makeRequest(String url){
    // ...
}
```

-----

At first glance, we'd want to write something like this;

```dart
// In an async block

final peopleInSpaceResponse = await makeRequest('http://api.open-notify.org/astros.json');
final peopleInSpace = jsonDecode(peopleInSpaceResponse.json);
final spaceImagesResponse = await makeRequest('https://domain.com/path');
final spaceImages = jsonDecode(spaceImagesResponse.json);

// Display in UI

```

The code above works as intended. but there is an opportunity to speed things up a little. 
Notice that while your program is making a network call to the `http://api.open-notify.org/astros.json` api,
your app simply `waits` for the result before proceeding to parse to json, only then will it make the second call to 
`https://domain.com/path` and then wait until a response is returned before proceeding.

The trick is, the second network call is totally independent of the first. You do not need the result of the first network 
call to make the second call, you only need both result so you can render stuff on your UI. Therefore, you can bundle all
*independent* asynchronous calls into one single call. This can be done in dart by passing the list
of all `Futures` as iterables to the `Future.wait` function as shown below;

```dart

final responses = await Future.wait([
    makeRequest('http://api.open-notify.org/astros.json'),
    makeRequest('https://domain.com/path')
]);

final peopleInSpace = jsonDecode(responses.first.json);
final spaceImages = jsonDecode(responses.last.json);

// Display in UI

```

*The above example can speed up code execution up to 2x, since it does not have to wait for an entire network call 
before making the second call* 

> Notice that Parallelling asynchronous operations only work with independent operations.


{{< figure src="parallel-async-ops.png" title="Linear vs Parallel" align="center" >}}


Thank You 🥳.

**Suggestions and Corrections are very welcome, Please comment below**