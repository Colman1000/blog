---
title: "Flutter Internationalization With GetX"
description: "Support multiple languages in flutter with GetX"
date: 2022-05-29T14:33:02+01:00
tags: ["flutter", "internationalization", "localization", "languages", "getx","android","ios"]
categories: ["Internationalization", "Localization", "Flutter"]
draft: false
author: "@Colman1000"
cover:
    image: "internationalization-cover.png"
    alt: "GetX Internationalization | Cover Image"
    caption: "Flutter Internationalization With GetX"
images: ['internationalization-cover.png', 'settings-dart-change-app-locale.jpg']
keywords: ["flutter", "internationalization", "localization", "languages", "getx","android","ios"]
summary: "GetX is a flutter package that tackles State management, Route management and Dependency management in a simple, powerful and efficient manner. In this article however, we'll look at how to implement Internationalization."
---

Internationalization/Localization is a must-have when your priority is to expand your reach. Building your app using 
Flutter is great, it allows you to reach more users by compiling to several platforms but supporting more than one language
is another way of increasing your reach.

> TL;DR: You can find the source code for this project [here](https://github.com/Colman1000/getx-internationalization)

With GetX, It is fairly easy to create an app with support for localizations. You only need X things;

- Create a class that *extends* the `Translations` class from `GetX` and override the `keys` property to 
represent the `Locales` you wish to support

- Add the entry for `translations` and `fallbackLocale` in the `GetMaterialApp` widget

- Use the localized strings in your UI.

Voila!

## Step I: Create Translations

This is the class you'd have to extend to *Internationalization/Localization* to work with GetX.
If you have tried localizing your app before, you would appreciate the ease with which GetX gets the job done.

Let's create a class, `Localizations` which extends `Translations`;

```dart
import 'package:get/get.dart';

class Localizations extends Translations {

  @override
  Map<String, Map<String, String>> get keys => {
        'en_US': {},
        'de_DE': {},
        'fr_FR': {},
      };
}

```
*localizations.dart*

As shown above, you'd have to override the `keys` field, which is a `Map`. this map would contain `Strings` as keys and
`Map<String, String>` as values.

The keys represent the language `Locales` in string format while the values are the actual translations. For example,
the `en_US` key would contain translations for *English, United States*.

An example `en_US` map would be as shown below

```dart
///...

'en_US' : {

    'app_name': 'Test App',
    'settings_text': 'Settings',
    'welcome_text': 'Hello, Welcome Aboard.',
     
}

///...

```

You'd simply have to repeat the same thing for other `Locales` you want to support and substitute the values with the
actual translations as shown below;

```dart
///...

'de_DE' : {

    // You can choose to leave a translation as is, if you want
    // the same word accross diffrent locales
     
    'app_name': 'Test App', 
    'settings_text': 'Einstellungen',
    'welcome_text': 'Hallo, Willkommen an Bord.',
     
}

///...

```

## Step II: Inject Translations into GetMaterialApp

For the translations we created in the previous step to work, we need to inject the `Localizations` class into 
`GetMaterialApp` and set up the default locale for your app.


```dart

void main() {
  runApp(
    GetMaterialApp(
      title: IntlKeys.title.tr,
      translations: Localizations(), //Instantiate and inject your translations here
      locale: Get.deviceLocale, // translations will be displayed in the locale selected here
      fallbackLocale: Intl.localeEN_US, // specify the fallback locale in case an invalid locale is selected.
      home: Container(),
    ),
  );
}

```

NB: the `locale` property specifies the Locale you want GetX to use by default. You can hard code a locale here
or use `GetX` to determine the current locale of the device your app is running on and use that.

Using the `Get.deviceLocale` option might be what you prefer, seeing that the app automatically uses the Locale the 
user's device is using.

This brings up an issue though, suppose you set the `locale` property to `Get.deviceLocale` but you do not support
`fr_FR` as a Locale. 😭

This is where the `fallbackLocale` comes in. You use this to select the locale to use if the selected locale is not supported.

## Step III: Use Translations

This is the easiest part; To use a translation, simply append `.tr` to the `keyString`. For example, suing the 
Localizations we defined above, we can get the translated `Settings` text anytime, anywhere in our app by using
`'settings_text'.tr`.

`.tr` is an extension method, included on the String class. simply import the GetX library by writing 
`import 'package:get/get.dart';` at the top of the page.

Below is an example for using the translated `settings_text` in your UI;

```dart

class App extends StatelessWidget {
  const App({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        // You can use the translated string as you would use any other string 
        child: Text('settings_text'.tr),
      ),
    );
  }
}

```

## Changing Locales

Our App now dynamically changes the text to the user's locale. **You can test this out by minimizing the app,
heading to settings, and changing the default language of your device.**

This is cool, but we might also want to give users the option to switch the App's language at any time.

For this, we can simply call the `updateLocale` method provide by GetX like shown below;

```dart
 ///...

 final _locale = Locale('de', 'DE');

 Get.updateLocale(_locale);

```

The example below illustrates how to add the option to change the Locale of the app in the settings page

```dart

class SettingsPage extends StatelessWidget {
  const SettingsPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final _textTheme = Get.textTheme;

    return Scaffold(
      appBar: AppBar(
        title: Text('settings_text'.tr),
      ),
      body: ListView(
        children: [
          Align(
            alignment: Alignment.centerLeft,
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Text(
                'select_app_language'.tr,
                style: _textTheme.headline6,
              ),
            ),
          ),
          const SizedBox(height: 16),
          ListTile(
            title: const Text('English'),
            dense: true,
            onTap: () {
              final _locale = Locale('en', 'US');

              Get.updateLocale(_locale);
            },
          ),
          ListTile(
            title: const Text('Deutsch'),
            dense: true,
            onTap: () {
              final _locale = Locale('de', 'DE');

              Get.updateLocale(_locale);
            },
          ),
          ListTile(
            title: const Text('한국인'),
            dense: true,
            onTap: () {
              final _locale = Locale('ko', 'KO');

              Get.updateLocale(_locale);
            },
          ),
          ListTile(
            title: const Text('Français'),
            dense: true,
            onTap: () {
              final _locale = Locale('fr', 'FR');

              Get.updateLocale(_locale);
            },
          ),
          const Divider(endIndent: 30, indent: 30),
          ListTile(
            title: Text('default_langage'.tr),
            dense: true,
            onTap: () {
              final _locale = Get.deviceLocale ?? Locale('en', 'US');

              Get.updateLocale(_locale);
            },
          ),
        ],
      ),
    );
  }
}

```

*settings.dart*

{{< figure src="settings-dart-change-app-locale.jpg" title="Settings Page for Changing App's Locale" align="center" >}}

## Advanced Concepts

GetX also allows for other niceties when localizing your app like;

### Passing *parameters* to the translated string:

There are two main types of parameters when using GetX translations, **Positional** and **Named** parameters 

For *positional* parameters, simply annotate a `%s` as the placeholder of your variable and call `.trArgs`, passing 
in a `List<String>` of strings as the replacements for the placeholders.

For *Named* parameters, Prepend an `@` symbol before the parameter name and call `trParams` with a `Map<String, String>`
that maps the parameters to the actual values

See the code below; 

```dart
import 'package:get/get.dart';


Map<String, Map<String, String>> get keys => {
    'en_US': {
        'logged_in_1': 'Hello %s, your email is %s',
        'logged_in_2': 'Hello @name, your email is @email',
    },
    'es_ES': {
       'logged_in_1': 'Hola %s, tu correo es %s',
       'logged_in_2': 'Hola @name, tu correo es @email',
    }
};

```

The actual values can be substituted as shown below

```dart


Text(
  'logged_in_1'.trArgs( [ 'John Eblabor', 'dblaq@example.com'])
);  /// EXPECTS :Hello John Eblabor, your email is dblaq@example.com

//OR


Text('logged_in_2'.trParams({
  'name': 'John Eblabor',
  'email': 'dblaq@example.com'
  },
)); /// EXPECTS :Hello John Eblabor, your email is dblaq@example.com

```


### Translating with singular and plural

For this example, consider the following translation

```dart
import 'package:get/get.dart';


Map<String, Map<String, String>> get keys => {
    'en_US': {
        'book_singular': 'I have a book',
        'book_positional_plural': 'I have %s books',
        'book_named_plural': 'I have @count books',
    },
   // ...
};

```

To make the translation respect singular and plural values, we'd use the `trPlural` or `trPluralParams`,
both of them take in a `String` argument as the *plural_key*, an `integer` for the *count* which outputs a `singular` for 
a value of **1** and `plural` for everything else, then lastly replacements for the placeholders.

The `trPlural` uses *Positional* arguments while the `trPluralParams` uses *Named* arguments.

See the code below:

```dart

// The base argument is always the singular_key,
// then we pass in the plural_key,
// then the count which determines whether to show a singular or plural variant
// lastly and optionally, pass in the replacement for any placeholders 
Text(
  'book_singular'.trPlural('book_positional_plural', 4, ['4'])
); //EXPECTS : I have 4 books

// OR

Text(
  'book_singular'.trPluralParams('book_positional_plural', 4, {'count': '4'})
); //EXPECTS : I have 4 books

///-------------------

// Changing the 4 to a 1 should output the singular variant
// Notice there is no placeholder in the singular translation and as such, nothing was replaced 😊
Text(
  'book_singular'.trPlural('book_positional_plural', 1, [1])
); //EXPECTS : I have a book

```


## Useful Tips

There are some useful tips to note while working with GetX translations

### Extract translations into separate files

This seems trivial, but it helps make your code cleaner and easier to work with.

Create separate files for each translation and include them in your translation class as shown below;

```dart
const en_US = <String,String>{
    'app_name': 'Test App',
    'settings_text': 'Settings',
    'welcome_text': 'Hello, Welcome Aboard.',
    //...
}
```
*translations/en_US.dart*

```dart
const de_DE = <String,String>{
    'app_name': 'Test App', 
    'settings_text': 'Einstellungen',
    'welcome_text': 'Hallo, Willkommen an Bord.',
    //...
}
```
*translations/de_DE.dart*

Then link them all in the Translation class like so;

```dart
import 'package:get/get.dart';

import 'translations/en_US.dart';
import 'translations/de_DE.dart';
// ...

class Localizations extends Translations {

  @override
  Map<String, Map<String, String>> get keys => {
        'en_US': en_US,
        'de_DE': de_DE,
        // ...
      };
}
```
*localizations.dart*

### Create a Static Class for your Translation Keys

It's very common to have a typo, instead of digging through your code to discover a typo, simple write a class that 
contains your translation keys and use that instead.

For example; This simple class would suffice

```dart
class IntlKeys {
    static const String appName = 'app_name';
    static const String settingsText = 'settings_text';
    static const String welcomeText = 'welcome_text';
    //...
}
```
*intl_keys.dart*

Then use this class as shown below:

```dart
Text( IntlKeys.settingsText.tr ); 
```

### Persisting User Selected Locales

Say we have completed and shipped our awesome app out to the world, Having our users go to the settings page each
time to select their preferred language is a terrible idea.

We can persist his selected `Locale` then use that the next time he visits... 😎


For this, we can use any package like [SharePreferences](https://pub.dev/packages/shared_preferences), 
[GetStorage](https://pub.dev/packages/get_storage) to save his preferred `Locale` as a string,

```dart
onTap(){
    ///... whenever a locale is selected
    
    final _locale =  Locale('de', 'DE');
    Get.updateLocale(_locale);
    _store.write('preferred_locale', _locale.toLanguageTag())
}
```

Good. 

When selecting the default locale in the `GetMaterialApp` try to read the `preferred_locale` and return the Locale or use 
the lovely `Get.deviceLocale`

```dart
//....
final _localeString = _store.read('preferred_locale');
final _locale = (_localeString == null || _localeString.isEmpty)
        ? Get.deviceLocale 
        : Locale.fromSubtags(_localeString)

//....
GetMaterialApp(
      title: IntlKeys.title.tr,
      translations: Intl(),
      locale:  _locale,
      fallbackLocale: Intl.localeEN_US,
    ),
  );
//....
```

## Conclusion

Localization is a useful and common thing for apps that are intended to reach a global audience. 
For such an important task, the implementation should not be difficult. As demonstrated above, I hope you see how GetX
makes Internationalization/Localization a breeze.

During your next project, try using GetX for localization and tell me how it went. I'd really love to hear from you.

Check out [this repo](https://github.com/Colman1000/getx-internationalization) for a working demo for structuring and implementing Internationalization/Localization in flutter
using GetX.
