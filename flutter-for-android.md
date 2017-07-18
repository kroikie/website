---
layout: page
title: Flutter for Android Developers
permalink: /flutter-for-android/
---
# ******Flutter for Android Developers******
Intro to flutter for android dev, what it is, etc

table of contents

# **Views** 

**What is the equivalent of a View in Flutter?**

In Android, the View is the foundation of everything that shows up on the screen. From Buttons, Toolbars and Inputs, everything is a View.  In Flutter the equivalent of a View would be Widget. However Widgets have a few differences when compared with a View. To start, widgets only live a few milliseconds on the screen (16ms/frame). In comparison, on Android when a View is drawn it does not redraw until invalidate is called. 

This is because unlike Android’s view hierarchy system where we have Views and we mutate those views on the view hierarchy, Widgets in Flutter are immutable, this allows Widgets to be super lightweight as they only have to remember their configuration. 

The question is, if widgets are immutable, how do I go about updating them? 

This is where the concept of Stateful vs Stateless widgets comes from. A StatelessWidget is just what it sounds like, a widget with no state information. StatelessWidgets are useful when the part of the user interface you are describing does not depend on anything other than the configuration information in the object.

If you want to dynamically change the the UI based on network data or user interaction then you have to work with StatefulWidget and tell the Flutter framework that the widget’s State has been updated so it can update that widget.

The important thing to note here is at the core both Stateless and Stateful widgets behave the same. They build and rebuild every frame, the difference is the StatefulWidget has a State object which stores state data across frames and restores it.

Let’s take a look at how you would use a StatelessWidget 

[code]

And how about a StatefulWidget with State

[code]



**How do I layout my Widgets? Where is my XML layout file?**

In Android you write layouts via XML, in Flutter you write your "layouts" building a widget tree. 
Lets look at an example of how you would display a simple Widget on the screen and add some padding to it.

In Android you would typically do 

[code]

in Flutter you would do

[code]

**How do I add or remove a Widget in Flutter?**

In Android you would call addChild or removeChild from a parent to dynamically add or remove views from a parent. In Flutter, because widgets are immutable there is no addChild, instead you can simply pass in a function that returns a widget to the parent and control that child's creation via a boolean.

For example:

[code] 

**In Android, I can Animate a view by View.animate(), how can I do that to a Widget?**
In Flutter animating widgets can be done easily via a handful of Animation Widgets. See https://flutter.io/widgets/animation/

Lets take a look at how to write a FadeTransition

[code]

**How do I build custom Widgets?**

Building custom widgets in Flutter is very similar to building custom Views in Android. You can extend the Widget and add your custom logic. 

Lets take a look at how to build a custom button

[code]

# **Intents**

**What is the alternative of an Intent in Flutter?**

Flutter does not have the concept of Intents. However in Android there are two main use-cases for Intents, One is to switch between Activities and another is to invoke external(or internal) components of your app (Camera, Services etc).

To switch between screens in Flutter you can directly access the router to draw a new Widget.

[code]

Now in Android you can pass some data along with the Intent, while the Navigation router does not have a way to pass data directly, you can approach this problem by creating a static “data” class that you can store and retrieve values when switching screens. 

There is also a very popular Router called “Fluro” that adds this functionality for you.

[code]

The other popular use-case for Intents is to call external components such as a Camera or File picker. For this you would need to create a native platform integration (or use an existing library)

See [Flutter Plugins] to learn how to build a native platform integration.



**How do I handle incoming Intents from external applications in Flutter?** 

This can be achieved by registering the intent you want to handle in the AndroidMainfest.xml to launch MainActivity (the default activity created by Flutter)

[code]

Then in MainActivity you can handle the intent and pass it to Flutter 

[code]

Lastly in Flutter you can receive this via

[code]

# **Async UI**

**What is the equivalent of runOnUiThread in Flutter?**

Because of Flutters reactive architecture, there is no need to worry about if you are running stuff on the UI thread, as long as you make sure you dont block your network calls.

For example when making long running calls you should always follow the async/await pattern

[code]

To update the UI you would call setState which would trigger the build method to run again and update the data.

[code]



**What is the equivalent of an AsyncTask or IntentService on Android?**

In many cases you do not have to worry about threading in Flutter as long as you follow the async/await patterns. 

Flutter built on Dart uses the same execution model of a single threaded event-loop, similar Node.js. To run code asynchronously you can declare the function as an async function and await on long running tasks in the function

[code]

This is where you would typically do network or database calls.

With regards to AsyncTask which follows a three step model onPreExecute, doInBackground and onPostExecute, doing a similar model in Flutter is much simpler. You can just write non-blocking code as it is, like you would in onPreExecute and onPostExecute and to run code asynchronously you would call an async function

[code]

onPostExecute typically takes the value from the result of the doInBackground task and updates the UI, in Flutter as you can see above you would just take the value returned from the await’ed function and update the UI by calling setState.

However there are times where you may be processing a large amount of data and your UI will hang. 

To get around this you can run code in the background. See <u>*How do I run parallel code on multi-core devices? I.e AsyncTask.execute(AsyncTask.THREAD_POOL_EXECUTOR) or run code in a background thread?*</u>



**How do I run parallel code on multi-core devices? I.e AsyncTask.execute(AsyncTask.THREAD_POOL_EXECUTOR) or run code in a background thread?**

Since Flutter uses a single threaded execution model, the code you write all executes on a single core on the mobile device. 

However it is possible to take advantage of multiple CPU cores to do long running or computationally intensive tasks. This is done by using Isolates.

Isolates are a separate execution thread that run and do not share any memory with the main execution thread. This means you can’t just access variables from the main thread or update your UI by calling setState. 

Lets see an example of a simple Isolate and how you can communicate and share data back to the main thread to update your UI.

[code]

While it’s no golden rule, try to keep your Isolates to the number of cpu cores on the device as having too many will cause your execution to slow down.



**How do I make network calls on Android and update the UI?**

Making a network call in Flutter is very easy when you use the popular "http" package from https://pub.dartlang.org/packages/http

You can use it by adding it to your dependecies in pubspec.yaml

[code]

Then to make a network call, for example to this JSON GIST on GitHub you can call

[code]

Once you have the result you can tell Flutter to update it's state by calling setState like so

[code]

Which will update your UI with the result from your network call.



**How do I show progress dialogs on android when there is a task that is running?**

Before you call your long running task, you can show a Dialog to let the user know there is processing happening. This can be done by rendering a Dialog widget. You can show the Dialog programatically by controlling it's rendering through a boolean and telling Flutter to update it's state just before your long running task.

[code]



# **Project Structure & Resources**

**Where do I store my resolution depedent image files?**

**Where do I store strings?**

**What is the alternative to Androids Resource system (i.e R.string, R.xml)**

**What is the alternative of a gradle file to add my external dependecies?**



# **Activities and Fragments**
 

# Lifecycles



# Layouts


# Gesture Detection and Event Handling


# Listviews & Adapters

 

# Working with Text



# Common Widget Map



# Form Input



# Drawing Shapes



# Flutter Plugins



# Themes

