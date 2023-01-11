# Android Components & Architecture

Activity - entry point for user interaction e.g. track what is on the screen

Service - run in the background e.g. music playback

Broadcast receivers - gateway for system event

Content providers - provide an API to share data with other apps, sharing can be done by URI

Intent - an asynchronous message which activates Activities, Services, and Broadcast receivers

Explicit Intent - specify which activity will handle this action

Implicit Intent - specify what action to be handled by activities

Sticky Intent: still exists after broadcasting, allowing other components to collect data

AndroidManifest.xml - declare existing components, API level, required permissions, required hardware features

`<intent-filter>` - specifies the types of intents components can respond to

Module - a collection of source files and build settings to divide the project for discrete functionality

jni/ - native code using the Java Native Interface (JNI)

gen/ - contains the Java files generated by Android Studio

assets/ - contains the file that should be compiled into an .apk file as-is. Can navigate this directory in the same way as a typical file system using URIs and read files as a stream of bytes using the AssetManager. A good location for textures and game data.

build.gradle - build configuration that applies to all modules

Context - context of the state of the app, get info about activity and application, access resources, preferences, room, both Activity, and Application extend Context

applicationContext: for reference by singleton class e.g. room, datastore

activityContext: for reference by UI operations e.g. toast, dialog

Application - contains all other components such as activities and services.

Fragment - has its layout and its behavior with its lifecycle callbacks, can add or remove fragments in the activity, can combine multiple fragments in an activity, can be reused in multiple activities, the life cycle is closely related to the lifecycle of its host activity

launchMode -

<sup>standard - default, will create a new activity regardless if it is in the task stack

<sup>singleTop - if the target activity is already on top, use it, otherwise, create a new one

<sup>singleTask - if the target activity is already in the task stack, use it and pop activities above it, otherwise create a new one

<sup>singleInstance - create a new activity in a new task

Fragment replace - removes existing fragment and adds new fragment

Fragment add - retains existing fragment and adds new fragment

using only the default Fragment constructor is recommended because Android calls a no-argument constructor and it is not aware of others

retained Fragment - By default, Fragments are destroyed and recreated along with their parent Activity’s when a configuration change occurs. Calling setRetainInstance(true) bypasses this cycle, retaining the current instance of the fragment when the activity is recreated.

When using fragment over activity: when UI components are reused across activities when showing views side by side

addToBackStack() - replace transaction is saved to back stack so the user can bring back the previous fragment with the back button

View - superclass for all UI components

ViewGroup - a parent to hold children e.g. LinearLayout with TextView and Button

Mipmap folder - for the launcher icon

When the user hits the system Back button, going from B back to A, the reverse happens: the entering destination A will have the popEnterAnim applied to it and the exiting destination B will have the popExitAnim applied to it.

MVC - +streamlines code -poor scalability -difficult unit-testing =view is not aware of controller, model is exposed to view

<sup>controller: user directly interacts with the controller

MVP - +easy unit-testing +view/presenter reusable +observer not required -additional view interface -coupled view/presenter =view has reference to presenter

<sup>presenter: communicates with a view via the interface

MVVM - +no coupled view/viewmodel +easy unit-testing -observe for each UI component -obsessive code -requires observer =view has reference to VM but VM is not aware of view so VM can be used in multiple views

AIDL: handles how the client and the service interact

Canvas: What to draw

Paint: How to draw

OkHttp vs Retrofit: Have to build requests yourself in OkHttp, Retrofit is slower on update because it is based on OkHttp

Gson vs Moshi: Moshi is written in Kotlin, is lighter, uses Kotlin code-gen instead of reflection

Why Moshi uses Kotlin codegen: it allows generating code at compile time instead of at runtime with reflection

activityViewModels(): injects viewmodel shared in the activity

Why use dependency injection?: easier refactoring, easier testing, easier to reuse code, easier collab, separate of concern

How DI works?: the service implements the interface, the client depends on the interface, the injector creates the service and injects it into the client

What Are Components In Dagger: They are a way of telling Dagger 2 what dependencies should be bundled together and made available to a given instance so they can be used.

What Are Modules In Dagger: installed to that component to allow binding to be accessed

Why use RecyclerView over ListView?: ListView creates as many views as data count in a dataset, with no built-support for animation, only vertical scroll

RecyclerView consists of:

<sup>Adapter: binding for a dataset, aware of where each item is located in the dataset

<sup>LayoutManager: positions items within the RecyclerView

<sup>ItemAnimator: defaults to DefaultItemAnimator

<sup>ViewHolder: draws for individual items

Mipmaps are used for icons, every resolution of them is used in case the launcher needs to display larger icon

Drawables are used for everything else, only 1 resolution will be used

TabLayoutMediator: links TabLayout and ViewPager2, synchronizing positions between them

Why does the Android App lag?: when GC occurs, your UI stops updating until GC is done. When there is too much done on the main thread

What is Garbage Collection?: reclaiming runtime memory by destroying unused objects

How does Garbage Collection work?: mark roots(objects referenced by static fields, application class, live thread)->mark referenced objects->mark reachable objects->GC unreachable objects

dp: abstract pixel unit scaled based on the dpi of the screen

sp: similar to dp except text size preference affects it too

Why use Jetpack?: a set of libraries that provide backward compatibility and best practices

R8: trims class/function name, remove unused methods, and merge code only used once

Low Memory Killer priority: empty(unused) processes->background processes(CREATED activity)->service processes->visible processes(STARTED activity)->foreground processes(RESUMED activity)

What to be cautious about while using Bitmap?: \TODO

How RecyclerView works?: The Adapter binds views and then passes them to the Layout Manager, the RecyclerView only allocates fixed numbers of views that fit the screen.

When a view is out of sight it becomes a scrap view and is temporary detached to the recycle pool, when the next items need to be displayed, it is reused by passing new data into the view and then is returned to the viewHolder as "dirty view"

ViewBinding vs DataBinding: DataBinding allows binding data to views while having a longer compile time

Why use clean architecture?: modularize the different functions of an application so that each component is separate and can be updated and tested independently

Domain: contains repeated or complex logic for a view model

Data: contains data handling logic

UI: contains UI handling logic

# Development

CI: build->test->merge CD: automatically deployment

Scrum: iterative development, daily meeting to report and adjust the process

ATDD: tests are written from the user's perspective "is this the result I expected?"

UML Diagram: is used to visualize the flow of a program [Link](https://www.smartdraw.com/uml-diagram/)

# Lifecycle

Activity:

- onCreate() - init activity, only called once through the whole lifecycle

- onStart() - when the user can see the screen

- onResume() - when the user can interact with the screen

- onPause() - when part of the app is visible but in the background

- onStop() - when the app is not visible to the user

- onDestroy() - activity is not longer used, clean up activity

if onStart() finish() is called, onPause() and onStop() won't be called and will just call onDestroy()

setContentView() is called in onCreate() because onCreate() is only called once

onSavedInstanceState() - store data before pausing the activity

onRestoreInstanceState() - recover the saved state of an activity when the activity is recreated after the destruction

Service:

<sup>onStartCommand(): runs when other components request the service to start by startService()

<sup>onBind(): when other Android components try to connect with the service

<sup>onCreate(): setup process when the service is created

<sup>onDestroy(): service is no longer used, clean up service

Fragment:

viewLifeCycleOwner: onViewCreated~onDestroyView

Fragment's LifecycleOwner: onCreate~onDestroy

<sup>onCreate() - initialize a fragment, when the fragment is added to a FragmentManager

<sup>onCreateView() - instantiate UI

<sup>onViewCreated() - gives subclasses a chance to initialize themselves once they know their view hierarchy has been completely created

<sup>onStart() - called when the fragment is visible, tied with activity's onStart()

<sup>onResume() - when the fragment is interactable, tied with activity's onResume()

<sup>onPause() - when the fragment is no longer interactable, tied with activity's onPause()

<sup>onStop() - when the fragment is no longer visible, tied with activity's onStop()

<sup>onDestroyView() - when the view onCreateView() created has been detached

<sup>onDestroy() - when the fragment is no longer used, when the fragment is removed from a FragmentManager

View:

<sup>onAttachedToWindow() - when the view is attached to a window

<sup>onMeasure() - determine the size of the view

<sup>onLayout() - called to assign size for its children if any

<sup>onDraw() - called when the view needs to render content

<sup>invalidate() - rerun from draw() process

<sup>requestLayout() - rerun from measure() process

# Kotlin/Java

inline function: inlined functions will not be actual functions in bytecode, instead the piece of code is part of the function used inline function, and will not be able to access private members/methods of your enclosing class. You will need to make those members/methods internal and then annotate them with @PublishedApi. Will be able to return from the lambda which in turn will return from the calling function.

crossinline: disallow return in lambda

noinline: noinline lambdas do not support non-local control flow, i.e you won’t be able to propagate your return to the calling function.

String concatenation: Strings are immutable in Java, so when a string is concatenated, it creates a new string and discards the old one

StringBuilder: mutable, asynchronous thus unsuitable for multi-threaded conditions, but is the fastest

StringBuffer: mutable, synchronous thus suitable for multi-threaded conditions, but is slower than StringBuilder

Kapt: generate annotation for Kotlin

Annotation: embed supplemental information to the source

Data Class: some utility functions are created by Kotlin if it is marked data class e.g. toString(), equals()/hashCode(), copy(), componentN(): destructure object to a variable

let: inputs object as a receiver, object as a parameter in lambda, returns the lambda result

run: inputs object as a receiver, object as a receiver in lambda, returns the lambda result

also: inputs object as a receiver, object as a parameter in lambda, returns the object

apply: inputs object as a receiver, object as a receiver in lambda, returns the object

with: inputs object as a parameter, object as a receiver in lambda returns the lambda result

Reflection is the ability to make modifications at runtime by making use of introspection.

List.associateBy: turns to a map

List.partition: turns to a pair by a predicate

@JvmStatic: marks the function as static in java, so you don't have to call that function by AppUtils.INSTANCE.install()

@JvmOverloads: java does not support default parameters, use this annotation to tell the compiler to create overloads for java

overloading: methods have the same name but different signatures e.g. parameters

@JvmField: tells the compiler not to generate getter/setter for the field

Integer vs int: Integer is a wrapper class of int which has utility functions to manipulate int, while int is a primitive type

List.flatMap is used to combining all the items of lists into one list

List.map is used to transform a list based on certain conditions

OOP: a concept about classes and objects which can be done in inheritance, polymorphism, abstraction, and encapsulation

you can implement another interface inside an interface

Polymorphism: different output from the same symbol in different inputs or different class

Inheritance: inherit the behavior of the parent class

How is String implemented?: it is a wrapper of an array of chars which is final

Why is String immutable?: to prevent manipulation of sensitive data

Auto/UnBoxing: auto/unwrapping primitive types to wrapper type

Memory Leak: unused objects GC is unable to clear

equals() vs ==: == compares reference, equals() compares value

Pair<T1, T2>: pair 2 data together so no need to declare a new data class for storing them

Triple<T1,T2,T3>: pair 3 data

Array in Kotlin: has fixed size, has better performance and is mutable so values can be changed

List in Kotlin: immutable by default unless created as MutableList, has to add/remove method to manipulate list size, has better scalability

== vs === in Kotlin: == checks value, === checks reference

label in Kotlin: used to declare which loop to break/continue in a for loop

sealed class: forces to add remaining branches while the referenced sealed class object is in when()

# Version Control

git fetch: brings my local copy of the remote repository up to date.

git pull: brings the changes in the remote repository to where I keep my own code.

# Design patterns

Singleton: single instance wherever it is accessed

Adapter: lets 2 classes work with each other without modifying their codes, done by interface conversion

Facade: simple interface to hide large code base

Factory Method: abstract factory and class to be inherited for various subclasses, used when a superclass has multiple subclasses, and the client does not need to care about the used subclass

Abstract Factory: creates families of related objects without instantiating directly, needing super factory for related factories

Prototype: clones object instead of constructing a new one, used when construction is time-consuming

Builder: input parameters one by one (build by steps), used to avoid instantiation of a class with many parameters, writing tests

Component: The component manages Leaves uniformly

Strategy: interface to handle different use cases

Observer: callback to its subscribers when value changes, used to achieve separation of concern

# Coroutine & Flow

CoroutineScope: the lifecycle of this coroutine

CoroutineContext: provides the required execution environment to run code asynchronously, consists of below:

<sup>CoroutineDispatcher: defines thread pools to run in.

<sup>CoroutineName

<sup>coroutineExceptionHandler

<sup>Job

CoroutineBuilders: helps create coroutines, can be called in non-suspending functions

job.join(): blocks the execution of the rest of the coroutine code before this job is finished

Coroutines.async: used when one needs to wait for the result, will block the main thread

Coroutines.launch: is set and forget.

tryEmit: tries to emit a value without suspending, can only return false if BufferOverflow strategy is suspended and subscribers are collecting shared flow

Job: its main responsibility is to manage coroutine cancellation.

withContext - switch thread to another in the coroutine scope.

supervisorScope/supervisorJob - if 1 thread fails it will continue to run.

StateFlow vs SharedFlow: Data rendered by the StateFlow (Text Composable) gets preserved after rotation. On the other hand, when using SharedFlow, the Toast does not get shown again after screen rotation. StateFlow supports .value, but does not collect repeated values, does not support replaying beyond the latest value, and requires an initial value.

GlobalScope: cannot be canceled

Mutex().withLock: protect all modifications of the shared state with a critical section that is never executed concurrently

transform: can emit arbitrary values many times

map: return result of transformed value

conflate: skip the emission if the collector can't process it in time

collectLatest: when the original flow emits a new value then the action block for the previous value is canceled

buffer: buffer the emission so the collector can process all while the source doesn't need to wait for the collector

take: cancel the execution of the flow when the corresponding limit is reached

reduce: Accumulates value starting with the first element and applying the operation to the current accumulator value and each element. Throws NoSuchElementException if the flow was empty.

fold: Accumulates value starting with initial value and applying operation current accumulator value and each element. Returns initial value if the collection was empty, can change the result type

zip: combines the corresponding values of two flows

combine: recompute whenever any of the upstream flows emit a value

flatMapLatest: cancel and recreate flow in the lambda if the original flow changes

flatMapConcat: concatenate original emitted flow execution

flatMapMerge: execute all emitted flow concurrently

cold flow: only starts emitting values when it is subscribed, values are not shared among other subscribers either

hot flow: starts emitting values no matter if it is subscribed or not, values are shared among subscribers

Why use StateFlow (over LiveData)?: has initial value and hence can be non-nullable, LiveData is dependant on the Android framework and is run on the main thread while StateFlow is multiplatform

Why use Flow (over Rx)?: has multiplatform support, suspending, enforced to be in scope, nullability support, and easier to write custom flows

WhileSubscribed cancels the upstream flow when there are no collectors

viewModelScope launches on the Main dispatcher by default

# Unit Test

3A: Arrange, Act, Assert

Why unit test?: identify defects early, enable code reuse, improve code readability, reduce refactor cost, quickly get feedback

# Network

gRPC: a framework for communication with Protocol Buffer

Channel: creates a connection to the server, required by Stub creation

Stub: calls methods defined in the proto files

Unlike JSON, however, protocol buffers are more than a serialized format. They include three other major parts:

<sup>- A contract definition language found in .proto files

<sup>- Generated accessor-function code

<sup>- Language-specific runtime libraries

gRPC is a new take on an old approach known as RPC (Remote Procedure Call), a form of inter-process communication essentially used for client-server interactions. The client can request to execute a procedure on the server

as if it were a normal local procedure call thanks to the stub generated for both client and server.

Multipart is a feature in most email clients that allows you to include multiple pieces of text in a single message

REST API: there are clients and a server, clients send HTTP request in methods GET/POST/PUT/DELETE to the server, and the server responds in a standard format, usually JSON

OSI Model:

- Layer 7 Application Layer: resource sharing or remote file access between applications e.g. HTTP

- Layer 6 Presentation Layer: translation of data e.g. encoding, data compression and encryption

- Layer 5 Session Layer: managing communication sessions

- Layer 4 Transport Layer: transmission of data using communication protocols e.g. TCP

- Layer 3 Network Layer: packet route selection and forwarding e.g. IP

- Layer 2 Data Link Layer: establishes and terminates a connection between two physically-connected nodes on a network

- Layer 1 Physical Layer: transmission of bit streams over a physical medium

# Programming basics

Program: executables stored in the storage

Process: executables running in the system

Thread: processes divided into multiple work units

Coroutine: switch to another thread while a processing thread is locking

Multi-tasking: do one more thing at a time

Multi-programming: load multiple programs into the system

Multi-threading: fast switching between threads

Synchronous: only process 1 task at a time

Asynchronous: can have more than 1 task ongoing at a time

Concurrent: 2 or more tasks are processed by 1 processor

Parallel: a task is broken into multiple subtasks processed by multiple processors

SOLID:

- S: single purpose per class

- O: open to extension but closed to modification

- L: replacing a superclass with its subclass shouldn't break any functions

- I: depending on the interface with needed functions rather than a concrete class with many functions, do not make functions useless to the client visible to it.

- D: high-level classes shouldn't depend on low-level classes, high-level class depends on an interface to avoid changing along with low-level classes

why floating numbers are inaccurate: because they can only hold this many bits after floating points, they will never accurately present an irrational number.

Interface: cannot have fields, can implement multiple interfaces, functions cannot be final. Used when functions are expected on unrelated classes when no changes are expected when designing small bits of functionality, can be inherited from another interface. "What can the object do"

Abstract class: can only inherit 1 abstract class, has constructors. Used when a base class is needed, when additional features are expected, inherence does not break superclass functions "What this object is"

Serialization: a process of translating a data structure or object state into a format that can be stored or transmitted

Serializable: stores data to file/disk.

Parcelable: faster than serializable, more complex to implement, won't be reflected(analyze the structure of the program)

Weak reference: referenced object does not need to stay in memory

Polymorphism: passing different parameters have different behaviors

Abstraction: exposes only what the object does e.g. abstract class and interface

Encapsulation: hides away implementation detail e.g. private, protected

Inheritance: inherit behaviors and info of the superclass

Database: an organized collection of data that is stored electronically

# DB

|| is a string concatenate operator. Think of it as + in Java String.

# LeetCode

BFS has no backtracking and uses a queue.

DFS uses the stack.

Why HashMap has a time complexity of O(1)?: traverses between hash strings instead of elements in the map.
