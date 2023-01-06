# Android Components & Architecture

Activity - entry point for user interaction e.g. track what is on the screen

Service - run in the background e.g. music playback

Broadcast receivers - gateway for system event

Content providers - provide an api to share data with other apps, sharing can be done by URI

Intent - an asynchronous message which activates Activities, Services and Broadcast receivers

Explicit Intent - specify which activity will handle this action

Implicit Intent - specify what action to be handled by activities

Sticky Intent: still exists after broadcasting, allowing other components to collect data

AndroidManifest.xml - declare existing components, API level, required permissions, required hardware features

`<intent-filter>` - specifies the types of intents components can respond to


Module - a collection of source files and build settings to divide project for discrete functionality

jni/ - native code using the Java Native Interface (JNI)

gen/ - contains the Java files generated by Android Studio

assets/ - contains file that should be compiled into an .apk file as-is. Can navigate this directory in the same way as a typical file system using URIs and read files as a stream of bytes using the AssetManager. A good location for textures and game data.

build.gradle - build configuration that applies to all modules

Context - context of state of the app, get info about activity and application, access resources, preferences, room, both Activity and Application extend Context

applicationContext: for reference by singleton class e.g. room, datastore

activityContext: for reference by UI operations e.g. toast, dialog

Application - contains all other components such as activities and services.

Fragment - has its layout and its behaviour with its lifecycle callbacks, can add or remove fragments in the activity, can combine multiple fragments in an activity, can be reused in multiple activities, life cycle is closely related to the lifecycle of its host activity

launchMode -

<sup>
standard - default, will create a new activity regardless if it is in the task stack
</sup>

<sup>
singleTop - if the target activity is already on top, use it, otherwise create new one
</sup>

<sup>
singleTask - if the target activity is already in the task stack, use it and pop activities above it, otherwise create new one
</sup>

<sup>
singleInstance - create a new activity in a new task
</sup>
Fragment replace - removes existing fragment and adds new fragment

Fragment add - retains existing fragment and adds new fragment

using only default Fragment constructor is recommended because Android calls no-argument constructor and it is not aware of others

retained Fragment - By default, Fragments are destroyed and recreated along with their parent Activity’s when a configuration change occurs. Calling setRetainInstance(true) bypasses this cycle, retaining the current instance of the fragment when the activity is recreated.

When use fragment over activity: when ui componments are reused across activities, when showing views side by side

addToBackStack() - replace transaction is saved to back stack so the user can bring back the previous fragment with back button

View - superclass for all UI components

ViewGroup - a parent to hold children e.g. LinearLayout with TextView and Button

Mipmap folder - for launcher icon

When the user hits the system Back button, going from B back to A, the reverse happens: the entering destination A will have the popEnterAnim applied to it and the exiting destination B will have the popExitAnim applied to it.

MVC - +streamlines code -poor scalability -difficult unit-testing =view is not aware of controller, model is exposed to view

<sup>
controller: user directly interacts with controller
</sup>

MVP - +easy unit-testing +view/presenter reusable +observer not required -additional view interface -coupled view/presenter =view has reference to presenter

<sup>
presenter: communicates with view via the interface
</sup>

MVVM - +no coupled view/viewmodel +easy unit-testing -observe for each UI component -obsessive code -requires observer =view has reference to vm but vm is not aware of view so vm can be used in multiple views

AIDL: handles how the client and the service interacts

Canvas: What to draw

Paint: How to draw

OkHttp vs Retrofit: Have to build requests yourself in OkHttp, Retrofit is slower on update because it is based on OkHttp

Gson vs Moshi: Moshi is written in Kotlin, is lighter, uses Kotlin code-gen instead of reflection

Why Moshi uses Kotlin codegen: it allows generating code at compile time instead of at runtime with reflection

activityViewModels(): injects viewmodel shared in the activity

Why use dependency injection?: easier refactoring, easier testing, easier to reuse code, easier collab, separate of concern

How DI works?: the service implements the interface, the client depends on the interface, the injector creates the service and injects it to the client

What Are Components In Dagger: They are a way of telling Dagger 2 what dependencies should be bundled together and made available to a given instance so they can be used.

What Are Modules In Dagger: installed to that component to allow binding to be accessed

Why use RecyclerView over ListView?: ListView creates as many views as data count in dataset, no built-support for animation, only vertical scroll

RecyclerView consists of:

<sup>
Adapter: binding for dataset, aware of where each items are located in the dataset
</sup>

<sup>
LayoutManager: positions items within the RecyclerView
</sup>

<sup>
ItemAnimator: defaults to DefaultItemAnimator
</sup>

<sup>
ViewHolder: draws for individual items
</sup>

Mipmaps are used for icons, every resolution of them are used in case the launcher needs to display larger icon

Drawables are used for everything else, only 1 resolution will be used

TabLayoutMediator: links TabLayout and ViewPager2, synchronizing positions between them

Why does Android App lag?: when GC occurs, your UI stops updating until GC is done. When there is too much done on main thread

What is Garbage Collection?: reclaiming runtime memory by destroying unused objects

How does Garbage Collection work?: mark roots(objects referenced by static fields, application class, live thread)->mark referenced objects->mark reachable objects->GC unreachable objects

dp: abstract pixel unit scaled based on dpi of the screen

sp: similiar to dp except text size preference affects it too

Why use Jetpack?: a set of libraries that provide backward compatability and best practices

R8: trims class/function name, remove unused methods and merge code only used once

Low Memory Killer priority: empty(unused) processes->background processes(CREATED activity)->service processes->visible processes(STARTED activity)->foreground processes(RESUMED activity)

What to be cauious about while using Bitmap?: \TODO

How RecyclerView works?: The Adapter binds views then pass them to the Layout Manager, the RecyclerView only allocates fixed numbers of views that fits the screen.

When a view is out of sight it becomes a scrap view and is temporary detached to the recycle pool, when next items need to be displayed, it is reused by passing new data into the view then is returned to the viewholder as "dirty view"

ViewBinding vs DataBinding: DataBinding allows binding data to views, while having longer compile time

Why use clean architecture?: modularize the different functions of an application so that each component is separate and can be updated and tested independently

Domain: contains repeated or complex logics for view model

Data: contains data handling logic

UI: contains UI handling logic

# Development

CI: build->test->merge CD: automatically deployment

Scrum: iterative development, daily meeting to report and adjust process

ATDD: tests are written from user's perspective "is this the result I expected?"

# Lifecycle

Activity:
<sup>
onCreate() - init activity, only called once through the whole lifecycle
</sup>

<sup>
onStart() - when user can see the screen
</sup>

<sup>
onResume() - when user can interact with the screen
</sup>

<sup>
onPause() - when part of app is visible but in background
</sup>

<sup>
onStop() - when app is not visible to user
</sup>

if onStart() finish() is called, onPause() and onStop() won't be called and will just call onDestroy()

setContentView() is called in onCreate() because onCreate() is only called once

onSavedInstanceState() - store data before pausing the activity

onRestoreInstanceState() - recover the saved state of an activity when the activity is recreated after destruction

Service:

<sup>
onStartCommand(): runs when other components requests the service to start by startService()
</sup>

<sup>
onBind(): when other Android components trying to connect with the service
</sup>

<sup>
onCreate(): setup process when the service is created
</sup>

<sup>
onDestroy(): service is no longer used, clean up service
</sup>

Fragment:
<sup>
viewLifeCycleOwner: onViewCreated~onDestroyView
</sup>

<sup>
Fragment's LifecycleOwner: onCreate~onDestroy
</sup>

<sup>
onCreate() - initialize a fragment, when the fragment is added to a FragmentManager
</sup>

<sup>
onCreateView() - instantiate UI
</sup>

<sup>
onViewCreated() - gives subclasses a chance to initialize themselves once they know their view hierarchy has been completely created
</sup>

<sup>
onStart() - called when the fragment is visible, tied with activity's onStart()
</sup>

<sup>
onResume() - when the fragment is interactable, tied with activity's onResume()
</sup>

<sup>
onPause() - when the fragment is no longer interactable, tied with activity's onPause()
</sup>

<sup>
onStop() - when the fragment is no longer visible, tied with activity's onStop()
</sup>

<sup>
onDestroyView() - when the view onCreateView() created has been detached
</sup>

<sup>
onDestroy() - when the fragment is no longer used, when the fragment is removed from a FragmentManager
</sup>

View:
<sup>
onAttachedToWindow() - when the view is attached to a window
</sup>

<sup>
onMeasure() - determine the size of the view
</sup>

<sup>
onLayout() - called to assign size for its children if any
</sup>

<sup>
onDraw() - called when the view needs to render content
</sup>

<sup>
invalidate() - rerun from draw() process
</sup>

<sup>
requestLayout() - rerun from measure() process
</sup>

# Kotlin/Java

inline function: inlined functions will not be actual functions in bytecode, instead the piece of code is part of function used inline function, will not be able to access private members/methods of your enclosing class. You will need to make those members/methods internal and then annotate them with @PublishedApi. Will be able to return from the lambda which in turn will return from the calling function.

crossinline: disallow return in lambda

noinline: noinline lambdas do not support non-local control flow, i.e you won’t be able to propagate your return to the calling function.

String concatenation: Strings are immutable in Java, so when a string is concatenated, it creates a new string and discards old one

StringBuilder: mutable, asynchronous thus unsuitable for multi-threaded condition, but is the fastest

StringBuffer: mutable, synchronous thus suitable for multi-threaded condition, but is slower than StringBuilder

Kapt: generate annotation for Kotlin

Annotation: embed supplemental information to the source

Data Class: some utility functions are created by Kotlin if it is marked data class e.g. toString(), equals()/hashCode(), copy(), componentN(): destructure object to variable

let: inputs object as receiver, object as parameter in lambda, returns the lambda result

run: inputs object as receiver, object as receiver in lambda, returns the lambda result

also: inputs object as receiver, object as parameter in lambda, returns the object

apply: inputs object as receiver, object as receiver in lambda, returns the object

with: inputs object as parameter, object as receiver in lambda, returns the lambda result

Reflection is the ability to make modifications at runtime by making use of introspection.

List.associateBy: turns to a map

List.partition: turns to a pair by predicate

@JvmStatic: marks the function as static in java, so you don't have to call that function by AppUtils.INSTANCE.install()

@JvmOverloads: java does not support default parameter, use this annotation to tell compiler to create overloads for java

overloading: methods have the same name but different signatures e.g. parameters

@JvmField: tells compiler not to generate getter/setter for the field

Integer vs int: Integer is a wrapper class of int which has utility functions to manipulate int, while int is a primitive type

List.flatMap is used to combine all the items of lists into one list

List.map is used to transform a list based on certain conditions

OOP: a concept that about classes and object which can be done in inheritance, polymorphism, abstraction, and encapsulation

you can implement another interface inside an interface

Polymorphism: different output from the same symbol in different input or different class

Inheritance: inherit the behavior of parent class

How is String implemented?: it is a wrapper of an array of chars which is final

Why is String immutable?: to prevent manipulation of sensitive data

Auto/UnBoxing: auto/unwrapping primitive types to wrapper type

Memory Leak: unused objects GC is unable to clear

equals() vs ==: == compares reference, equals() compares value

Pair<T1,T2>: pair 2 data together so no need to declare a new data class for storing them

Triple<T1,T2,T3>: pair 3 data

Array in Kotlin: has fixed size, has better performance, is mutable so values can be changed

List in Kotlin: inmutable by default unless created as MutableList, has add/remove method to manipulate list size, has better scalability

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

Factory Method: abstract factory and class to be inherited for various subclasses, used when a superclass has multiple subclasses, and client does not need to care about used subclass

Abstract Factory: creates families of related objects without instantiating directly, needing super factory for related factories

Prototype: clones object instead of constructuring new one, used when construction is time-consuming

Builder: input parameter one by one (build by steps), used to avoid instantation of a class with many parameters, writing tests

Component: Component manages Leaves uniformly

Strategy: interface to handle different use cases

Observer: callback to its subscribers when value changed, used to achieve separation of concern


# Coroutine & Flow

CoroutineScope: lifecycle of this coroutine

CoroutineContext: provides required execution environment to run code asynchronously, consists of below:

<sup>
CoroutineDispatcher: defines thread pools to run in.
</sup>

<sup>
CoroutineName
</sup>

<sup>
coroutineExceptionHandler
</sup>

<sup>
Job
</sup>

CoroutineBuilders: helps creating coroutines, can be called in non-suspending functions

job.join(): blocks the execution of rest of coroutine code before this job is finished

Coroutines.async: used when one needs to wait for the result, will block main thread

Coroutines.launch: is set and forget.

tryEmit: tries to emit a value without suspending, can only return false if BufferOverflow strategy is suspended and subscribers are collecting shared flow

Job: its main responsibility is to manage coroutine cancellation.

withContext - switch thread to another in the coroutine scope.

supervisorScope/supervisorJob - if 1 thread fails it will continue to run.

StateFlow vs SharedFlow: Data rendered by the StateFlow (Text Composable) gets preserved after rotation. On the other hand, when using SharedFlow, the Toast does not get shown again after screen rotation. StateFlow supports .value,but does not collect repeated values, does not support replaying beyond the latest value, requires initial value.

GlobalScope: cannot be cancelled

Mutex().withLock: protect all modifications of the shared state with a critical section that is never executed concurrently

transform: can emit arbitrary values many times

map: return result of transformed value

conflate: skip the emission if the collector can't process it in time

collectLatest: when the original flow emits a new value then the action block for the previous value is cancelled

buffer: buffer the emission so the collector can process all while the source doesn't need to wait for the collector

take: cancel the execution of the flow when the corresponding limit is reached

reduce: Accumulates value starting with the first element and applying operation to current accumulator value and each element. Throws NoSuchElementException if flow was empty.

fold: Accumulates value starting with initial value and applying operation current accumulator value and each element. Returns initial value if the collection was empty, can change result type

zip: combines the corresponding values of two flows

combine: recompute whenever any of the upstream flows emit a value

flatMapLatest: cancel and recreate flow in the lambda if original flow changes

flatMapConcat: concatate original emitted flow execution

flatMapMerge: execute all emitted flow concurrently

cold flow: only starts emitting values when it is subscribed, values are not shared among other subscribers either

hot flow: starts emitting values no matter it is subscribed or not, values are shared among subscribers

Why use StateFlow (over LiveData)?: has initial value hence can be non-nullable, LiveData is dependant on Android framework and is run on main thread while StateFlow is multiplatform

Why use Flow (over Rx)?: has multiplatform support, suspending, enforced to be in a scope, nullability support, easier to write custom flows

WhileSubscribed cancels the upstream flow when there are no collectors

viewModelScope launches on Main dispatcher by default


# Unit Test

3A: Arrange, Act, Assert

Why unit test?: identify defects early, enable code reuse, improve code readability, reduce refactor cost, quickly get feedback


# Network

gRPC: framework for communication with Protocol Buffer

Channel: creates connection to the server, required by Stub creation

Stub: calls methods defined in the proto files

Unlike JSON, however, protocol buffers are more than a serialized format. They include three other major parts:

<sup>
- A contract definition language found in .proto files
</sup>

<sup>
- Generated accessor-function code
</sup>

<sup>
- Language-specific runtime libraries
</sup>

gRPC is a new take on an old approach known as RPC (Remote Procedure Call), a form of inter-process communication essentially used for client-server interactions. The client can request to execute a procedure on the server
as if it were a normal local procedure call thanks to the stub generated for both client and server.

Multipart is a feature in most email clients that allows you to include multiple pieces of text in a single message

REST API: there are clients and a server, clients send http request in methods GET/POST/PUT/DELETE to the server, the server responses in a standard format, usually JSON


# Programming basics

Program: executables stored in storage

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

S: single purpose per class

O: open to extension but closed to modification

L: replacing a superclass with its subclass shouldn't break any functions

I: depended on interface with needed functions rather than a concrete class with many functions, do not make functions useless to the client visible to it.

D: high-level classes shouldn't depend on low-level classes, high-level class depends on an interface to avoid changing along with low-level classes

why floating numbers are inaccurate: because it can only hold this many bits after floating points, it will never accurately present an irrational number.

Interface: cannot have fields, can implement multiple interfaces, functions cannot be final. Used when functions are expected on unrelated classes, when no changes are expected, when designing small bits of functionality, can be inherit from another interface. "What can the object do"

Abstract class: can only inherit 1 abstract class, has constructors. Used when a base class is needed, when additional features are expected, inherence does not break superclass functions "What this object is"

Serialization: process of translating a data structure or object state into a format that can be stored or transmitted

Serializable: stores data to file/disk.

Parcelable: faster than serializable, more complex to implement, won't be reflected(analyze structure of the program)

Weak reference: referenced object does not need to stay in memory

Polymorphism: passing different parameters have different behaviors

Abstraction: abstracting away the implementation details of a class and only presenting a clean, easy-to-use interface via the class’s member functions

Encapsulation: hides data from unnecessary access, exposes only necessary data/functions

Inheritance: inherit behaviors and info of the superclass


# DB

|| is string concatenate operator. Think of it as + in Java String.


# Leetcode

BFS has no backtracking, uses queue.

DFS uses stack.

Why HashMap has time complexity of O(1)?: traverses between hash string instead of elements in the map
