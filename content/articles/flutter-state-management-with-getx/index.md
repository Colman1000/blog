---
title: "Flutter State Management With GetX"
description: "Introducing GetX: Flutter State Management With GetX"
date: 2022-05-15T11:43:13+01:00 
tags: ["flutter","getx","android","ios","state management",]
categories: ["state management", "flutter"]
draft: false
images: []
keywords: ["flutter","getx","android","ios","state management"]
summary: "GetX is a package that tackles State management, Route management and Dependency management in a simple, powerful and efficient manner. It also comes with helper utilities for simplifying Internationalization, Theme management, making HTTP requests, Validators and so much more."
---

State management in Flutter is *hot* topic everyone likes to talk about. There are many contenders in this category, the
likes of [Provider](https://pub.dev/packages/provider), [Riverpod](https://pub.dev/packages/riverpod)
, [Redux](https://pub.dev/packages/redux), [BLoC](https://pub.dev/packages/bloc), [MobX](https://pub.dev/packages/mobx)
and so on. For this article however, we would be taking a look at the [GetX package](https://pub.dev/packages/get).

## What is GetX?

GetX is a package that tackles **State management**, **Route management** and **Dependency management** in a simple,
powerful and efficient manner. It also comes with helpers utilities for simplifying
*Internationalization*, *Theme management*, making *HTTP requests*, *Validators* and so much more.

The highlights of GetX include the following;

#### Write less code:

Many other state management solutions for flutter have the bottle-neck of *boiler-plate code*; i.e. redundant code
developers are forced to write over and over again. To reduce this hurdle, other state management solutions
utilize `code generation`, a technique that automatically generates the boiler-plate code for the developer.

With GetX however, you do not need to worry about boiler-plate or generated code. Simply extend the `GetxController` and
write your code like a boss.

#### Performance & Efficiency:

GetX observables (*reactive values*) are built using the low latency `GetValue` and `GetStream` classes from GetX. Hence
there is no buffering, to very low memory consumption. The GetX controllers also auto dispose themselves when no they
are longer in use. This makes for an efficient and performant application even on low-end devices.

#### Simplicity:

Another benefit to using GetX is the ease at which Reactive programming is accomplished; It's as easy as `setState`.
Simply create a variable and append `.obs` to make it observable;

```dart
var name = 'John Eblabor'.obs;
```

Then in your UI,when you want to show that value and update the screen whenever the values changes, simply do this:

```dart
Obx(() => Text("${controller.name}"));
```

Simple and short.

## Managing State

GetX offers several methods of handling state from simple, single value ephemeral state to complex, app-wide state.
Whichever you'd pick depends on your use case.

### Ephemeral State Management:

GetX provides options to help you handle single value state in a simple and elegant manner. These can be used in cases
where you simply need to toggle the `obscureText` property in a TextField to reveal/hide the value, changing the current
index for a
`BottomNavigationBar`, toggle the visibility of a Widget or even change the `Image.Network` child of a Container based
on a single value.

### ValueBuilder

This is a simplified version of a `StatefulWidget`. It aims to simplify verbosity of the writing a single value Widget.

To use the `ValueBuilder` Widget, simply create the widget and give it a type as shown below and give it
an `initialValue`.

*NB: If you ever use a `ChangeNotifier` or a `StreamController`  as the value of the builder, the value is automatically
disposed* 😎

```dart

ValueBuilder<int>(
  initialValue: 0,
  builder: (value, onUpdate) {
    return BottomNavigationBar(
      items: const [
        /*..BottomNavigationBarItems go here*/
      ],
      onTap: (v) {
        onUpdate(v); // this calls setstate to update the selected index
      },
    );
  },
  // if you need to call something outside the builder method.
  onUpdate: (value) => print("Value updated: $value"),
  onDispose: () => print("Widget unmounted"),
)

```

### ObxValue

This is similar to the

[ValueBuilder]({{< relref "#valuebuilder" >}} "ValueBuilder") only that instead of a regular value like an `int`
or `String`, you pass a `Rx instance` ( *as discussed [here]({{< relref "#getxcontroller" >}} "GetxController")* ) and
the widget updates automatically when the value changes.

```dart
ObxValue((data) {
  return Switch(
    value: data.value,
    onChanged: data, // Rx has a _callable_ function! 
    //You could also use (flag) => data.value = flag,
  );
},
  false.obs,
)
```

### App State Management:

There are several ways to handle app-wide state using GetX, This kind of state refers to global data shared across
several widgets or screens scattered all over your app.

Before we talk about this section; Let me introduce you to ... 🥁 ... `GetxController`

### GetxController

GetX provides a neat way to separate your business logic from your View or UI elements. You can completely remove
StatefulWidget by using GetxController since it has `onInit` and `onClose` methods which serve same functions as
the `initState` and `dispose` in a `StatefulWidget`.

When your controller is created in memory, the `onInit` method is called immediately, and the `onClose` method is called
when it is removed from memory.

There is another method, the `onReady` method, which is called after the widget has been rendered on the screen.

Let's consider the Flutter Counter App implement with GetX;

```dart
class CounterController extends GetxController {
    @override
  void onInit() {
    // Write code you would otherwise write in an `initState` override of a stateful widget
    // for example, making an API call or instantiating animation controllers
    super.onInit(); 
  }
  
  @override
  void onClose() {
    // Write the code you would for the `dispose` override of a stateful widget
    // for example, cancel timers or disposing objects like extEditingControllers, 
    // AnimationControllers to avoid memory leaks 
    super.onInit();
  }
  
  @override
  void onReady() {
    // Write the code to run when you UI is built and ready. Technically, this is 
    // called 1 frame after onInit(); It's a place to write run events, 
    // like snackbars, dialogs or even also make async request.
    super.onReady();
  }
}

```

#### Reactive Variables

To use make the `GetxController` useful, we need to create *observable or reactive*  variables. These variables help to
inform your widgets when their value changes so the widgets can re-render.

There are several ways to create observable variables in GetX;

#### 1. GetX Helpers

GetX provides helpers for dart primitive types to easily create observables. These include the non-nullable `RxInt`
, `RxBool`, `RxString`, `RxList` `RxMap` and their nullable counterparts `RxnInt`, `RxnBool`, `RxnString` etc.

Example;

```dart
var counter = RxInt(0);
var isName = RxnString(null);
```

#### 2. Using Rx and Dart Generics

Asides the helpers GetX provides, you can create an observable using the generic type of the variable. This is the
method you would likely use for your custom types or classes.

Example;

```dart
var counter = Rx<int>(0);
var isName = Rx<String?>(null);
```

#### 3. Shorthand

Finally, GetX provides a short form for creating observables, simply append `.obs` to the variable. Voila!

Example;

```dart
var counter = 0.obs;
var isName = ''.obs;
```

Whatever method you use is fine.

**NB: Quick Note on `Observable` or `Reactive` variables**

```dart

final name = 'John Eblabor'.obs;

//to change the value of name, we use the following line of code
name.value = 'Hey';

//NB: name only "updates" the stream, if the new value is different from the current one.

// All Rx properties are "callable" and returns the new value.
// but this approach does not accepts `null`, the UI will not rebuild.
name('Hello');

// is like a getter, prints 'Hello'.
name() ;

final abc = [0,1,2].obs;
// Converts the value to a json Array, prints RxList
// Json is supported by all Rx types!
print('json: ${jsonEncode(abc)}, type: ${abc.runtimeType}');

// RxMap, RxList and RxSet are special Rx types, that extends their native types.
// but you can work with a List as a regular list, although is reactive!
abc.add(12); // pushes 12 to the list, and UPDATES the stream.
abc[3]; // like Lists, reads the index 3.


/// Custom Rx Models:
// toJson(), toString() are deferred to the child, 
// so you can implement override on them, and print() the observable directly.

class User {
    String name, last;
    int age;
    User({this.name, this.last, this.age});

    @override
    String toString() => '$name $last, $age years old';
}

final user = User(name: 'John', last: 'Doe', age: 33).obs;

// `user` is "reactive", but the properties inside ARE NOT!
// So, if we change some variable inside of it...
user.value.name = 'Roi';
// The widget will not rebuild!,
// `Rx` don't have any clue when you change something inside user.
// So, for custom classes, we need to manually "notify" the change.
user.refresh();

// or we can use the `update()` method!
user.update((value){
  value.name='Roi';
});

print( user );

```

### Using observable variables

Consider the following code as the `MyController` for our Counter App.

```dart
class MyController extends GetxController {
  
  // Create a variable that can notify other widgets when its value changes
  // by simply appending `.obs` to the value 
  final count = 0.obs;
  
  void increment(){
    // Note the `.value` on the count variable as mentioned above
    count.value += 1; // count is of type Rx<int>  
  }
  void decrement(){
    count.value -= 1;
  }  
}
```

To utilize our controller, we can either use the `Obx` or the `GetBuilder` Widgets which listen to changes from our
observable variables and updates its children.

The `Obx` is very useful when using GetX's `Dependency Manager` alongside the `State Management` or when the controller
is already instantiated.

#### GetBuilder

The following example is how the to re-render the UI using `GetBuilder`

```dart
class Counter extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        GetBuilder<MyController>(
          init: MyController(),
          /* initialize MyController if you use 
          it first time in your views */
          builder: (controller) {
            return IconButton(
              icon: const Icon(Icons.add),
              onPressed: controller.increment,
            );
          },
        ),
        GetBuilder<MyController>(
          /* No need to initialize MyController again here, since it is 
          already initialized in the previous GetBuilder */
          builder: (controller) {
            return Text('${controller.count.value}');
          },
        ),
        GetBuilder<MyController>(
          builder: (controller) {
            return IconButton(
              icon: const Icon(Icons.remove),
              onPressed: controller.decrement,
            );
          },
        ),
      ],
    );
  }
}

```

#### Obx

`Obx` implementation

```dart

class Counter extends StatelessWidget {

// Controller needs to be initialized before use with Obx
final _ = MyController(); 

  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        IconButton(
          icon: const Icon(Icons.add),
          onPressed: _.increment,
        ),
        Obx(()=> Text('${_.count.value}')),
        IconButton(
          icon: const Icon(Icons.remove),
          onPressed: _.decrement,
        ),
      ],
    );
  }
}


```

### StateMixin

GetX also provides a `StateMixin` class that helps clean up asynchronous task representation.

Say we are building an application that needs to fetch data from the backend. We might need a way to easily track the
state (*loading*, *error*, *success*) and the data returned from the server in our controller and use if-else statements
to determine the UI to render on the screen.

GetX's also got us covered with the `StateMixin`. This helps us keep track of our status at any point in the app using a
clean API. You simply need to include `StateMixin` by using the `with` keyword. for example;

```dart
class DataController extends GetxController with StateMixin<ServerData> {}
```

`StateMixin` also provides the `RxStatus` class to help keep track of the state of the controller at any point and also
provides the `change` method to change the `RxStatus`. All you need to do is pass the new state and the status.

```dart
change(newState, status: RxStatus.success());
```

Checkout the code below

```dart
class DataController extends GetxController with StateMixin<ServerData>{

    @override
    void onInit() {
        fetchProducts();
        super.onInit();
    }


    // You can  fetch data from remote server
    void fetchProducts() async {
        // Show the loading indicator
        change(null, status: RxStatus.loading());   
        final response = await fetchDataFromRemoteServer(); 

        If(response.hasData) {
            final data = ServerData.fromMap(response.data);
            // Successfully fetched products data
            change(data, status: RxStatus.success());     

          } else if(response.hasError) {
              // Error occurred while fetching data
          change(null, status: RxStatus.error('Something went wrong')); 

          } else {
              // No products data
          change(null, status: RxStatus.empty());
        }
    }
}

```

To utilize this controller, simply use the `obx` Widget from the `GetxController`

```dart

class RemoteDataPage extends StatelessWidget {

    final controller = DataController();

    @override 
    Widget build(BuildContext context) {
        return Scaffold(
            body: controller.obx(

                (state) => ShowProductList(state),

                onLoading: AppLoader(),

                onEmpty: Text('No Data'),

                onError: (error) => Text(error),

            )
        );
    }
}

```

The `controller.obx` widget will change your UI according to the changes of the status and the state. 🐱‍🏍

## Conclusion

This article does not cover the entirety of GetX. It is simply aimed at providing a quick overview on how GetX makes you
a happier developer with its unique approach to `State Management`.

Watch out for the next article on `Dependency Management`, `Route Management` & `Helpers` in GetX.

You can read more about GetX from [official documentation](https://pub.dev/packages/get).
