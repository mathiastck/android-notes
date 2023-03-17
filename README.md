# Android

## Components

Activity: entry point for user interaction e.g. track what is on the screen

Service: run in the background e.g. music playback

Broadcast receivers: gateway for system event

Content providers: provide an API to share data with other apps, sharing can be done by URI

Intent: an asynchronous message which activates Activities, Services, and Broadcast receivers

- Explicit Intent: specify which activity will handle this action

- Implicit Intent: specify what action to be handled by activities

- Sticky Intent: still exists after broadcasting, allowing other components to collect data

Module: a collection of source files and build settings to divide the project for discrete functionality

`build.gradle`: build configuration that applies to all modules

`AndroidManifest.xml`: declare existing components, API level, required permissions, required hardware features

- `<intent-filter>`: specifies the types of intents components can respond to

- launchMode:

    - standard: default, will create a new activity regardless if it is in the task stack

    - singleTop: if the target activity is already on top, use it, otherwise, create a new one

    - singleTask: if the target activity is already in the task stack, use it and pop activities above it, otherwise create a new one

    - singleInstance: create a new activity in a new task

Context: context of the state of the app, get info about activity and application, access resources, preferences, room, both Activity, and Application 
Context

`applicationContext`: for reference by singleton class e.g. room, datastore

`activityContext`: for reference by UI operations e.g. toast, dialog

Application: contains all other components such as activities and services.

Fragment: has its layout and its behavior with its lifecycle callbacks, can add or remove fragments in the activity, can combine multiple fragments in an activity, can be reused in multiple activities, the life cycle is closely related to the lifecycle of its host activity

- Fragment replace: removes existing fragment and adds new fragment

- Fragment add: retains existing fragment and adds new fragment

- Retained Fragment: By default, Fragments are destroyed and recreated along with their parent Activity’s when a configuration change occurs. Calling setRetainInstance(true) bypasses this cycle, retaining the current instance of the fragment when the activity is recreated.

- When to use fragment over activity: when UI components are reused across activities when showing views side by side

- `addToBackStack()`: replace transaction is saved to back stack so the user can bring back the previous fragment with the back button

- When the user hits the system Back button, going from B back to A, the reverse happens: the entering destination A will have the popEnterAnim applied to it and the exiting destination B will have the popExitAnim applied to it.

- Using only the default Fragment constructor is recommended because Android calls a no-argument constructor and it is not aware of others

Mipmap folder: for the launcher icon

`jni/`: native code using the Java Native Interface (JNI)

`gen/`: contains the Java files generated by Android Studio

`assets/`: contains the file that should be compiled into an .apk file as-is. Can navigate this directory in the same way as a typical file system using URIs and read files as a stream of bytes using the AssetManager. A good location for textures and game data.

AIDL: handles how the client and the service interact

ViewBinding vs DataBinding: DataBinding allows binding data to views while having a longer compile time

## Architecture

MVC:

- +streamlines code

- -poor scalability

- -difficult unit-testing

- view is not aware of controller, model is exposed to view

- controller: user directly interacts with the controller

MVP:

- +easy unit-testing

- +view/presenter reusable

- +observer not required

- -additional view interface

- -coupled view/presenter

- =view has reference to presenter

- presenter: communicates with a view via the interface

MVVM:

- +no coupled view/viewmodel

- +easy unit-testing

- -observe for each UI component

- -obsessive code

- -requires observer

- =view has reference to VM but VM is not aware of view so VM can be used in multiple views

Why use clean architecture?: modularize the different functions of an application so that each component is separate and can be updated and tested independently

- Domain: contains repeated or complex logic for a view model

- Data: contains data handling logic

- UI: contains UI handling logic

Event-driven architecture:

- +loosely coupled, subscriber does not need to know who sends the event

- +easy to pass simple data for update

- -difficult debugging, harder to trace what subscriber handles the event

## API

OkHttp vs Retrofit: Have to build requests yourself in OkHttp, Retrofit is slower on update because it is based on OkHttp

Gson vs Moshi: Moshi is written in Kotlin, is lighter, uses Kotlin code-gen instead of reflection

Why Moshi uses Kotlin codegen: it allows generating code at compile time instead of at runtime with reflection

## View

`View`: superclass for all UI components

`ViewGroup`: a parent to hold children e.g. `LinearLayout` with `TextView` and Button

`Canvas`: What to draw

`Paint`: How to draw

How `RecyclerView` works?: The Adapter binds views and then passes them to the Layout Manager, the RecyclerView only allocates fixed numbers of views that fit the screen.  When a view is out of sight it becomes a scrap view and is temporary detached to the recycle pool, when the next items need to be displayed, it is reused by passing new data into the view and then is returned to the viewHolder as "dirty view"

Why use `RecyclerView` over `ListView`?: `ListView` creates as many views as data count in a dataset, with no built-support for animation, only vertical scroll

`RecyclerView` consists of:

- `Adapter`: binding for a dataset, aware of where each item is located in the dataset

- `LayoutManager`: positions items within the RecyclerView

- `ItemAnimator`: defaults to DefaultItemAnimator

- `ViewHolder`: draws for individual items

Mipmaps are used for icons, every resolution of them is used in case the launcher needs to display larger icon

Drawables are used for everything else, only 1 resolution will be used

`TabLayoutMediator`: links TabLayout and ViewPager2, synchronizing positions between them

`dp`: abstract pixel unit scaled based on the dpi of the screen

`sp`: similar to dp except text size preference affects it too

`ConstraintLayout` helpers:

- Chains: a group of views that are linked to each other with bi-directional position constraints, can be used to distribute space between views evenly, eliminating the necessity of `LinearLayout`, they have different modes:

    - `spread`: default mode, it will position the views in the chain at even intervals within the available space
    
    <img src="resources/chain_spread.png" alt="chain spread picture"/>
    
    - `spread_inside`: snaps the outermost views in the chain to the outer edges, and then positions the remaining views in the chain at equal intervals within the available space

    <img src="resources/chain_spread_inside.png" alt="chain spread inside picture"/>

    `layout_constraintHorizontal_weight` / `layout_constraintVertical_weight` can be used to adjust a view's "weight" in its width/height if its `layout_width`/`layout_height` is set to 0 and is in `spread` or `spread_inside` chains

    - `packed`: packs the views together, and then centres the group within the available space, the positioning of the packed views can be further controlled by altering the `bias` value
    
    <img src="resources/chain_packed.png" alt="chain packed picture"/>
    
- `Barrier`: allows you to specify a constraint based on the height/weight of target views, which makes a `Barrier` flexible

- `Guldeline`: specifies a fixed distance in dp or in percent based on the layout's size

- `Flow`: a virtual layout which is able to wrap other views without adding a level to the layout hierarchy

- `Layer`:

- `Group`:

## Dependency Injection

`activityViewModels()`: injects viewmodel shared in the activity

Why use dependency injection?: easier refactoring, easier testing, easier to reuse code, easier collab, separate of concern

How DI works?: the service implements the interface, the client depends on the interface, the injector creates the service and injects it into the client

Why use Dagger?:

- easier dependency management

- allowing to pass mocked dependencies from the outside for easier unit testing

- easier scope management

What Are Components In Dagger?: They are a way of telling Dagger 2 what dependencies should be bundled together and made available to a given instance so they can be used.

What Are Modules In Dagger?: installed to that component to allow binding to be accessed

## Performance

Why does the Android App lag?: when GC occurs, your UI stops updating until GC is done. When there is too much done on the main thread

What is Garbage Collection?: reclaiming runtime memory by destroying unused objects

How does Garbage Collection work?: mark roots(objects referenced by static fields, application class, live thread)->mark referenced objects->mark reachable objects->GC unreachable objects

Why use Jetpack?: a set of libraries that provide backward compatibility and best practices

R8: trims class/function name, remove unused methods, and merge code only used once

Low Memory Killer priority: empty(unused) processes->background processes(CREATED activity)->service processes->visible processes(STARTED activity)->foreground processes(RESUMED activity)

What to be cautious about while using Bitmap?: Downscale the bitmap or load the lower resolution version of the bitmap to fit into the view, so it does not take up unnecessary memory for no benefit

Memory Leak: unused objects GC is unable to clear

## Lifecycle

Activity:

- `onCreate()`: init activity, only called once through the whole lifecycle

- `onStart()`: when the user can see the screen

- `onResume()`: when the user can interact with the screen

- `onPause()`: when part of the app is visible but in the background

- `onStop()`: when the app is not visible to the user

- `onDestroy()`: activity is no longer used, cleaning up activity

if in `onStart()`, `finish()` is called, `onPause()` and `onStop()` won't be called and will just call `onDestroy()`

`setContentView()` is called in `onCreate()` because `onCreate()` is only called once

`onSavedInstanceState()`: store data before pausing the activity

`onRestoreInstanceState()`: recover the saved state of an activity when the activity is recreated after the destruction

Service:

- `onStartCommand()`: runs when other components request the service to start by `startService()`

- `onBind()`: when other Android components try to connect with the service

- `onCreate()`: setup process when the service is created

- `onDestroy()`: service is no longer used, clean up service

Fragment:

Lifecycle of `viewLifeCycleOwner`: `onViewCreated()`~`onDestroyView()`

Lifecycle of `LifecycleOwner` of the Fragment: `onCreate()`~`onDestroy()`

- `onCreate()`: initialize a fragment, when the fragment is added to a `FragmentManager`

- `onCreateView()`: instantiate UI

- `onViewCreated()`: gives subclasses a chance to initialize themselves once they know their view hierarchy has been completely created

- `onStart()`: called when the fragment is visible, tied with `onStart()` of the activity

- `onResume()`: when the fragment is interactable, tied with `onResume()` of the activity

- `onPause()`: when the fragment is no longer interactable, tied with `onPause()` of the activity

- `onStop()`: when the fragment is no longer visible, tied with `onStop()` of the activity

- `onDestroyView()`: when the view `onCreateView()` created has been detached

- `onDestroy()`: when the fragment is no longer used, when the fragment is removed from a `FragmentManager`

View:

- `onAttachedToWindow()`: when the view is attached to a window

- `onMeasure()`: determine the size of the view

- `onLayout()`: called to assign size for its children if any

- `onDraw()`: called when the view needs to render content

`invalidate()`: rerun from `draw()` process

`requestLayout()`: rerun from `measure()` process

## Debug

adb: a CLI tool used to communicate with a device, can be used to debug or install apps

App Inspection: can be used to inspect the in-app database and network usage

Profiler: enables tracking CPU, memory and energy usage with events shown in a timeline

# Development

CI: build->test->merge CD: automatically deployment

Scrum: iterative development, daily meeting to report and adjust the process

ATDD: tests are written from the user's perspective "is this the result I expected?"

UML Diagram: is used to visualize the flow of a program [Link](https://www.smartdraw.com/uml-diagram/)

# Programming basics

SOLID:

- S: single purpose per class

- O: open to extension but closed to modification

- L: replacing a superclass with its subclass shouldn't break any functions

- I: depending on the interface with needed functions rather than a concrete class with many functions, do not make functions useless to the client visible to it.

- D: high-level classes shouldn't depend on low-level classes, high-level class depends on an interface to avoid changing along with low-level classes

Interface: cannot have fields, can implement multiple interfaces, functions cannot be final. Used when functions are expected on unrelated classes when no changes are expected when 
ing small bits of functionality, can be inherited from another interface. "What can the object do"

Abstract class: can only inherit 1 abstract class, has constructors. Used when a base class is needed, when additional features are expected, inherence does not break superclass functions "What this object is"

Serialization: a process of translating a data structure or object state into a format that can be stored or transmitted

Serializable: stores data to file/disk

Parcelable: faster than serializable, more complex to implement, won't be reflected(analyze the structure of the program)

Weak reference: referenced object does not need to stay in memory

OOP: a concept about classes and objects which can be done in inheritance, polymorphism, abstraction, and encapsulation

Polymorphism: passing different parameters have different behaviors

Abstraction: exposes only what the object does e.g. abstract class and interface

Encapsulation: hides away implementation detail e.g. private, protected

Inheritance: inherit behaviors and info of the superclass

## Kotlin/Java

inline function: inlined functions will not be actual functions in bytecode, instead the piece of code is part of the function used inline function, and will not be able to access private members/methods of your enclosing class. You will need to make those members/methods internal and then annotate them with `@PublishedApi`. Will be able to return from the lambda which in turn will return from the calling function.

- `crossinline`: disallow return in lambda

- `noinline`: `noinline` lambdas do not support non-local control flow, i.e you won’t be able to propagate your return to the calling function.

`@JvmStatic`: marks the function as static in java, so you don't have to call that function by AppUtils.INSTANCE.install()

`@JvmOverloads`: java does not support default parameters, use this annotation to tell the compiler to create overloads for java

`@JvmField`: tells the compiler not to generate getter/setter for the field

Reflection is the ability to make modifications at runtime by making use of introspection.

`Integer` vs `int`: `Integer` is a wrapper class of int which has utility functions to manipulate `int`, while `int` is a primitive type

You can implement another interface inside an interface

Auto/UnBoxing: auto/unwrapping primitive types to wrapper type

`equals()` vs `==`: `==` compares reference in Java, `equals()` compares value based on how the class implements

`==` vs `===` in Kotlin: `==` checks `equals()`, `===` checks reference

`Pair<T1, T2>`: pair 2 data together so no need to declare a new data class for storing them

`Triple<T1,T2,T3>`: pair 3 data

`Array` in Kotlin: has fixed size, has better performance and is mutable so values can be changed

`List` in Kotlin: immutable by default unless created as `MutableList`, has to add/remove method to manipulate list size, has better scalability

label in Kotlin: used to declare which loop to break/continue in a for loop

Overloading: methods have the same name but different signatures e.g. parameters

Kapt: generate annotation for Kotlin

Annotation: embed supplemental information to the source

`data class`: some utility functions are created by Kotlin if it is marked data class:

`sealed class`: forces to add remaining branches while the referenced sealed class object is in when()

- `toString()`

- `equals()`/`hashCode()`

- `copy()`

- `componentN()`: destructure object to a variable

`let()`: inputs object as a receiver, object as a parameter in lambda, returns the lambda result

`run()`: inputs object as a receiver, object as a receiver in lambda, returns the lambda result

`also()`: inputs object as a receiver, object as a parameter in lambda, returns the object

`apply()`: inputs object as a receiver, object as a receiver in lambda, returns the object

`with()`: inputs object as a parameter, object as a receiver in lambda returns the lambda result

### Coroutines

`CoroutineScope`: the lifecycle of this coroutine

`Job`: its main responsibility is to manage coroutine cancellation.

- `join()`: blocks the execution of the rest of the coroutine code before this job is finished

`CoroutineContext`: provides the required execution environment to run code asynchronously, consists of below:

- `CoroutineDispatcher`: defines thread pools to run in.

- `CoroutineName`

- `CoroutineExceptionHandler`

- `Job`

`CoroutineBuilders`: helps create coroutines, can be called in non-suspending functions

`Coroutines.async()`: used when one needs to wait for the result, will block the main thread

`Coroutines.launch()`: is set and forget.

`withContext()`: switch thread to another in the coroutine scope.

`supervisorScope`/`supervisorJob`: if 1 thread fails it will continue to run.

`GlobalScope`: cannot be canceled

`Mutex().withLock()`: protect all modifications of the shared state with a critical section that is never executed concurrently

### Flow

cold flow: only starts emitting values when it is subscribed, values are not shared among other subscribers either

hot flow: starts emitting values no matter if it is subscribed or not, values are shared among subscribers

Why use StateFlow (over LiveData)?: has initial value and hence can be non-nullable, LiveData is dependant on the Android framework and is run on the main thread while StateFlow is multiplatform

Why use Flow (over Rx)?: has multiplatform support, suspending, enforced to be in scope, nullability support, and easier to write custom flows

`SharingStarted.WhileSubscribed()` cancels the upstream flow when there are no collectors

`viewModelScope` launches on the Main dispatcher by default

`Flow.tryEmit()`: tries to emit a value without suspending, can only return false if BufferOverflow strategy is suspended and subscribers are collecting shared flow

`StateFlow` vs `SharedFlow`: Data rendered by the `StateFlow` (Text Composable) gets preserved after rotation. On the other hand, when using `SharedFlow`, the `Toast` does not get shown again after screen rotation. `StateFlow` supports `StateFlow.value`, but does not collect repeated values, does not support replaying beyond the latest value, and requires an initial value.

Flow functions:

- `transform()`: can emit arbitrary values many times

- `map()`: return result of transformed value

- `conflate()`: skip the emission if the collector can't process it in time

- `collectLatest()`: when the original flow emits a new value then the action block for the previous value is canceled

- `buffer()`: buffer the emission so the collector can process all while the source doesn't need to wait for the collector

- `take()`: cancel the execution of the flow when the corresponding limit is reached

- `reduce()`: Accumulates value starting with the first element and applying the operation to the current accumulator value and each element. Throws NoSuchElementException if the flow was empty.

- `fold()`: Accumulates value starting with initial value and applying operation current accumulator value and each element. Returns initial value if the collection was empty, can change the result type

- `zip()`: combines the corresponding values of two flows

- `combine()`: recompute whenever any of the upstream flows emit a value

- `flatMapLatest()`: cancel and recreate flow in the lambda if the original flow changes

- `flatMapConcat()`: concatenate original emitted flow execution

- `flatMapMerge()`: execute all emitted flow concurrently

### String

String concatenation: `String`s are immutable in Java, so when a `String` is concatenated, it creates a new `String` and discards the old one

`StringBuilder`: mutable, asynchronous thus unsuitable for multi-threaded conditions, but is the fastest

`StringBuffer`: mutable, synchronous thus suitable for multi-threaded conditions, but is slower than StringBuilder

How is `String` implemented?: it is a wrapper of an array of chars which is final

Why is `String` immutable?: to prevent manipulation of sensitive data

### List function

- `associateBy()`: turns to a map

- `partition()`: turns to a pair by a predicate

- `flatMap()` is used to combining all the items of lists into one list

- `map()` is used to transform a list based on certain conditions


# Version Control

`git fetch`: brings my local copy of the remote repository up to date.

`git pull`: brings the changes in the remote repository to where I keep my own code.

# Design patterns

## Creational

- Singleton: single instance wherever it is accessed

- Factory: dedicate the creation method to the superclass, while allowing subclasses to override the method, used when a superclass has multiple subclasses, and the client does not need to care about the used subclass

- Abstract Factory: creates families of related objects without instantiating directly, needing super factory for related factories

- Prototype: clones object instead of constructing a new one, used when construction is time-consuming

- Builder: input parameters one by one (build by steps), used to avoid instantiation of a class with many parameters, writing tests

## Structural

- Adapter: lets 2 classes work with each other without modifying their codes, done by interface conversion

- Facade: simple interface to hide large code base

- Composite: The component manages Leaves uniformly

- Proxy: a proxy disguises itself as the actual service, enabling it to take control of the service without the client knowing, you can also replace a proxy easily because it implements the same interface as the service does, used when the duplication of the target object is expensive e.g. database

## Behavioral

- Strategy: interface to handle different use cases

- State: state is an extension of the strategy pattern, it allows states to manipulate the context of other state

- Observer: callback to its subscribers when value changes, used to achieve separation of concern

- Mediator: this pattern forces components to collaborate via the mediator, components extend the mediator interface, while the concrete mediator object holds references to all components, components are not aware of each other, used to reduce dependencies

# Unit Test

3A: Arrange, Act, Assert

Why unit test?: identify defects early, enable code reuse, improve code readability, reduce refactor cost, quickly get feedback

# Network

gRPC: a framework for communication with Protocol Buffer

Channel: creates a connection to the server, required by Stub creation

Stub: calls methods defined in the proto files

Unlike JSON, however, protocol buffers are more than a serialized format. They include three other major parts:

- A contract definition language found in .proto files

- Generated accessor-function code

- Language-specific runtime libraries

gRPC is a new take on an old approach known as RPC (Remote Procedure Call), a form of inter-process communication essentially used for client-server interactions. The client can request to execute a procedure on the server

as if it were a normal local procedure call thanks to the stub generated for both client and server.

Multipart is a feature in most email clients that allows you to include multiple pieces of text in a single message

REST API: there are clients and a server, clients send HTTP request in methods GET/POST/PUT/DELETE to the server, and the server responds in a standard format, usually JSON

OSI Model: Describes how each communication protocol works together to enable communication between 2 devices

- Layer 7 Application Layer: resource sharing or remote file access between applications e.g. HTTP

- Layer 6 Presentation Layer: translation of data e.g. encoding, data compression and encryption

- Layer 5 Session Layer: managing communication sessions

- Layer 4 Transport Layer: transmission of data using communication protocols e.g. TCP

- Layer 3 Network Layer: packet route selection and forwarding e.g. IP

- Layer 2 Data Link Layer: establishes and terminates a connection between two physically-connected nodes on a network

- Layer 1 Physical Layer: transmission of bit streams over a physical medium

# Computer Science

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

Why floating numbers are inaccurate: because they can only hold this many bits after floating points, they will never accurately present an irrational number.

# DB

An organized collection of data that is stored electronically

`||` is a string concatenate operator. Think of it as `+` in Java String.

# LeetCode

BFS has no backtracking and uses a queue.

DFS uses the stack.

Why HashMap has a time complexity of O(1)?: traverses between hash strings instead of elements in the map.

# System Design

Steps of system designing:

1. Define the problem: System design problem is usually ambiguous, so communicate with your interviewers.

2. Design high-level: Start from overviewing the whole system, then go into the detail.

3. Dive deep: Highlight the core components and constraints.

4. Find bottlenecks: Solve scalability problem

5. Summarize and answer questions: Summarize design choices and elaborate if the choices justify for the tradeoffs

HTTP: Primary protocol used to transfer data between a web server and a client.

HTTPS: Secured version of HTTP

TCP/IP: Connects devices over the internet for data transmission.

Load Balancing: Distribute workloads to multiple servers, so that the addition of server is possible.

Content Delivery Networks: A network of servers that delivers content to the user based on their location, the purpose is to maintain content availability and to improve user experience (reduced latency).

CAP Theorem: A principle that states a distributed database system cannot guarantee all three of the following properties simultaneously.

- Consistency

- Availability

- Partition tolerance

Caching: Store frequently accessed data in a temporary storage location to improve time spent on retrieving data.
