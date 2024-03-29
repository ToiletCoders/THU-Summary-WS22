# Mobile Application Development

## General

multiple choice questions, naming, explaining, analyzing/writing source code

- Know concepts, names. Be able to explain these as precise as possible.
- Be able to explain every kind of code on the topics from this course.
- Be able to write simple code. However, I do not expect you to be able to write code
for complex API’s on paper (e.g. for creating notifications, exact source code for
custom adapters...).
- Refresh your knowledge how you realized various aspects of the projects “Currency
Converter”, “Compass” and “MAD Music Player”

## Question Collection

### Overview: Mobile Computing

- **Which kinds of mobility can be distinguished in Mobile Computing?**
  - Device mobility - mobile and networked
  - User mobility - user is mobile and uses different devices
  - Service mobility - service is available everywhere and anytime
- **What does the term “convergence” mean?**
  - mobile and stationary devices become increasingly similar
- **Name 5 different mobile operating systems!**
  - iOS
  - Android
  - Windows Phone/Windows 10 Mobile
  - Blackberry OS
  - Firefox OS
  - (future) Fuchsia
- **From a business perspective, does it make sense to always focus on
developing for the platform(s) with the largest market share?**
  - No, because the market share is not the only important factor. Other factors
    are the target group, the development costs, the time to market, the
      development skills, the market share of the competitors, etc.
- **Compared to developing desktop applications, what are special aspects of
developing mobile applications? Name 5!**
  - limited screen size, touch gestures
  - usage is often not continuous
  - less resources available
  - power dissipation is important
  - potentially slower network connection
  - many sensors available
- **During lecture, you encountered two fundamental approaches to app
development. Name them and discuss the differences!**
  - Native Development Tools (or Native SDKs)
    - Pros: full access to all features of the device, high performance
    - Cons: development for each platform separately, high development costs
  - Cross-Platform Development Tools
    - pros: development for multiple platforms with one code base, low development costs
    - cons: limited access to device features, lower performance

### First Steps

- **Shown is the Android architecture (F2.7). Briefly explain or name the 6
different areas!**
  - System Apps (Dialer, Email, Browser, etc.)
  - Java API Framework (Content Providers, View System, Managers, etc.)
  - Native Libraries (C/C++ Libraries)
  - Android Runtime (ART, Core Libraries)
  - Hardware Abstraction Layer (HAL) (Audio, Sensors, etc.)
  - Linux Kernel (Drivers, Power Management, etc.)
- **What do the terms “Minimum SDK” and “Target SDK” mean for an Android
project?**
  - Minimum SDK: the lowest Android version that is supported by the app
  - Target SDK: the Android version that the app is developed for
- **How can you make an Android device ready to use for development?**
  - Activate Developer Options
  - Connect the device to the computer
  - Allow USB debugging
- **What is an activity?**
  - App component that is responsible for a single screen with a user interface
  - can start other activities
  - <-- returns to the previous activity
- **Given a basic project structure (AndroidManifest.xml, res/activity_main.xml)
give an implementation for the activity MainActivity that shows the layout!**

```java
package com.example.ZSF;
import ...;
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

- **How can you add an event handler to a button? (describe with source code or
natural language)**
  - add an onClick attribute to the button in the layout and create the method
  - set an onClickListener in the activity

```java
Button button = (Button) findViewById(R.id.button);
button.setOnClickListener(v -> {
            (// do something)
        });
```

- **How can you find a View from your layout to access its properties / call
methods? (source code AND explanation)**
- Search for the View in the `R` class (e.g. `R.id.button`)

```java
Button button = (Button) findViewById(R.id.button); // cast is unnecessary
```

### App Contents / UI

- **Given an AndroidManifest.xml explain its elements! (application, usespermission,
activity, intent-filter, service…)**
  - Declares information about the app and its services
  - Required permissions ```<uses-permission android:name="android.permission.CALL_PHONE/>```
  - SDK version ```<uses-sdk android:minSdkVersion="15" android:targetSdkVersion="25"/>```
  - All activities ```<activity android:name=".MainActivity">```
  - Services ```<service android:name=".MyService"/>```
  - Broadcast receivers ```<receiver android:name=".MyReceiver"/>```
  - Content providers ```<provider android:name=".MyProvider"/>```
- **Why do we have to declare required permissions in our manifest?**
  - The user has to explicitly grant the permission
- **How did the handling of permissions change with Android 6.0 (API level 23)?
(Moodle FAQ)**
  - Originally had to be granted by the user on installation
  - Since Android 6.0, the user has to grant permissions at runtime
- **What is the “Back-Stack”?**
  - Stack (last in first out) of the past sequence of Activities
- **How is the relationship between View and ViewGroup? Name an example
concrete class for each!**
  - View: a single UI element (e.g. Button, TextView, ImageView)
  - ViewGroup: a container for other Views (e.g. LinearLayout, RelativeLayout)
- **Name 3 rules for creating good mobile user interfaces from a user experience
point of view!**
  - Don't clutter screens: One Activitry per screen
  - Easy to understand arragements
  - Intuitive usability
  - perform longer running tasks in the background
  - standard keys keep their standard functions
  - no special menu entry "Exit App"
- **How can you connect a ListView with its data?**
  - Use an Adapter (e.g. ArrayAdapter)
- **What do you have to provide to an ArrayAdapter when creating it (in natural
language, exact parameter order not relevant)**
  - Context (Activity), Layout ressource, Array with data, id of the TextView
- **When will an ArrayAdapter not be enough, meaning you have to implement a
custom adapter class?**
  - If you want to display more than one View per item
  - Use a different layout
- **Given a custom adapter class (like F3.36), explain what happens inmethod
getView()!**
  - gets data element at position ``i``
  - Creates view, reuses existing view when provided to the method
  - Fills view with data

```java
public View getView(int i, View view, ViewGroup viewGroup) {
    if (view == null) {
        view = LayoutInflater.from(context).inflate(R.layout.list_item, viewGroup, false);
    }
    TextView textView = (TextView) view.findViewById(R.id.textView);
    textView.setText(data[i]);
    return view;
}
```

### Ressources / Event Handling / Toolbars

- **What is a resource qualifier? Name 3 examples!**
  - Language
  - Region
  - Orientation
  - Resolution, display density
  - day/night
- **How can you access a resource from within your application?**
  - f.e. ```R.string.app_name``` or ```R.drawable.ic_launcher```
- **Which view can be used to show a picture?**
  - ``ImageView``
- **How can you create a multilingual app for Android?**
  - Use ``strings.xml`` for each language with different language qualifiers
- **Which basic steps do have to take if you want to add a toolbar menu/icons to
your app? (only basic steps!)**
  - Remove standard title bar in the manifest ``android:theme="@style/Theme.AppCompat.Light.NoActionBar">``
  - Add toolbar to the layout
  - Set default toolbar in onCreate

```java
Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
setSupportActionBar(toolbar);
```

### Intents

- **What are two fundamental types of intents?**
  - Explicit intent: via and ID (e.g. ``new Intent(this, SecondActivity.class)``) 
  - Implicit intent: give an action type or a data type (e.g. ``new Intent(Intent.ACTION_VIEW, Uri.parse("http://www.google.com"))``)
- **Given an empty onClickHandler and a second activity SecondActivity, invoke
this activity! (source code)**

```java
public void onClick(View view) {
    Intent intent = new Intent(this, SecondActivity.class);
    startActivity(intent);
}
```

- **How can you pass data with an intent? (source code)**

```java
Intent intent = new Intent(this, SecondActivity.class);
intent.putExtra("key", "value");
startActivity(intent);
```

- **What is the basic control flow when we expect a result from an invoked
activity (f.e. camera)? (basic explanation)**
  - Start activity with ``startActivityForResult(intent, requestCode)``
  - Override ``onActivityResult(requestCode, resultCode, data)`` in the activity
  - Check if the result is valid and get the data

### Web APIs

- **What is a “Web API”?**
  - Application Programming Interface for the Web
  - Provides access to data and functionality of an application
  - Can be used by other applications
- **What is REST?**
  - Representational State Transfer
  - Architectural style for distributed hypermedia systems
  - Uses HTTP for communication
- **What are the most common formats for computer consumable information in
Web APIs?**
  - XML, JSON
- **Given a (uncommented) list of required classes/methods query a given web
ressource (f.e. OMDB url) and extract information from the resulting XML
(example given) (source code!)**

```java	
private void updateCurrencies() {
    Thread thread = new Thread(() -> {
        try {
            String webString = "https://www.ecb.europa.eu/stats/eurofxref/eurofxref-daily.xml";
            try {
                URL url = new URL(webString);
                URLConnection connection = url.openConnection();
                XmlPullParser parser = XmlPullParserFactory.newInstance().newPullParser();
                parser.setInput(connection.getInputStream(), connection.getContentEncoding());
                int eventType = parser.getEventType();
                while (eventType != XmlPullParser.END_DOCUMENT) {
                    if (eventType == XmlPullParser.START_TAG) {
                        if (parser.getName().equals("Cube")) {
                            String currency = parser.getAttributeValue(null, "currency");
                            String rate = parser.getAttributeValue(null, "rate");
                            if (currency != null && rate != null) {
                                ExchangeRateDatabase.setRates(currency, rate);
                            }
                        }
                    }
                    eventType = parser.next();
                }
            } catch (Exception e) {
                Log.e("ErrorURL", "Error with XML: " + e.getMessage());
                throw new RuntimeException(e);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    });
    thread.start();
}
```

- **Why will we get a NetworkOnMainThreadException if we try to access the
internet directly from the event handler?**
  - Because the main thread is not allowed to do network operations
  - Problems:
    - GUI would block (single thread)
    - Access to network data could potentially take a long time
    - If it takes too long Android will kill the app

### App Lifecycle / Sensors

- **In which situation is onCreate / onResume / on Pause / onDestroy called?**
  - ``onCreate``: when the activity is created
  - ``onResume``: when the activity is resumed
  - ``onPause``: when the activity is paused
  - ``onDestroy``: when the activity is destroyed/finished
- **When is an apps process (and all its threads) destroyed?**
  - When the app is closed
  - When the app is in the background for a long time
- **Name 2 examples of each a base sensor and a composite sensor!**
  - Base sensors: Accerleration, Gyroscope, Proximity, Air pressure, Light
  - Composite sensors: Acceleration, Gravity, Step counter
- **What are the steps to take if you want to obtain sensor values for a compass?
(basic steps)**
  - Get sensor manager
  - Get sensor
  - Register listener

```java
SensorManager sman = (SensorManager) getSystemService(Context.SENSOR_SERVICE);
List<Sensor> sensors = sman.getSensorList(Sensor.TYPE_LIGHT);
if(sensors.size() == 0) {
    // no sensor found
} else {
    Sensor sensor = sensors.get(0);
    sman.registerListener(this, sensor, SensorManager.SENSOR_DELAY_NORMAL);
}
```

### Concurrency / Notifications / Deployment

- **What does concurrency on Android have in common with concurrency in
Java?**
  - Threads
  - Runnable
  - synchronized
- **When do we need concurrency in Android apps?**
  - When we want to do something in the background
  - When we want to do something in parallel
  - When we want to do something in the UI thread
- **If we started a new thread, why can’t we update the UI (progress, results…)
directly from there?**
  - Views can only be changed from UI thread!
  - ``runOnUiThread`` can be used to run code on UI thread
- **Name two ways to send code to execute to the UI thread! (method names /
code example with runnable)**
  - ``runOnUiThread``
  - ``post``

```java
runOnUiThread(new Runnable() {
    @Override
    public void run() {
        // code to run on UI thread
    }
});
```

- **Name three ways an Android app can notify the user!**
  - Toast
  - Notification
  - Dialog
- **What are the elements of a Notification?**
  - Title, text, icon, sound, vibration, light, priority, category, visibility, actions
- **What are the basic steps to create a Notification?**
  - Get access to NotificationManager
  - Create Notification Channel
  - Prepare Notification Builder
  - Create intent for action to be invoked when notification is tapped and add it to Notification Builder
  - Show / update notification
  - Remove notification
- **Why do we need to sign apps with a certificate?**
  - To authenticate author for updates - not during initial installation
- **Which malicious scenario does app signing prevent?**
  - TODO

### Data Storage

- **Name 3 options for persisting data in your app!**
  - Shared Preferences
  - Access File System
  - Embedded SQLite Database
  - Content Provider
  - Paper.io
- **How is file access different in Android from standard Java?**
  - Access to two different types of storage locations - internal and external
  - External
    - Requires special permission ```<uses-permission android:name=”android.permission.WRITE_EXTERNAL_STORAGE”/>```
    - Does not have to be available
  - Internal
    - Access using Activity methods
- **Where do you have to store files if you do not want other apps be able to
access them?**
  - in the Internal Storage
- **Can you store sensitive data on external storage? Why?**
  - No, because it is not encrypted
  - Storage is accessible by other apps
  - Can be removed or made unavailable by the user
- **What is SQlite? How is it conceptually different from other DBMS like MySQL,
Oracle or postgresql?**
  - SQLite is a relational database management system contained in a C library
  - It is a serverless, zero-configuration, transactional SQL database engine
  - It is the most widely deployed SQL database engine in the world
- **To access an SQlite database we wrote a class extending SQLiteOpenHelper?
What is its purpose?**
  - Configure initialization and upgrade of database
  - Gives access
- **What is a “Content Provider”? Name examples!**
  - It is a standard interface that allows you to access application-specific data
  - Contacts, Calendar, Media, SMS, Email, Browser, Downloads, Settings, Call Log
- **Given some Content Provider code, can you explain?**
  - Slide 8.22

### Services / Audio

- **What is a Service?**
  - A service is a component that runs in the background to perform long-running operations
  - f.e. playing music, downloading files, synchronizing data, etc.
- **When would you use an IntentService, when a JobService?**
  - IntentService: starts from an intent, runs in background, stops itself when done, runs independently
  - JobService: automatically starts when conditions are met, runs in background, stops itself when done, runs independently
- **If you use a service that is part of the operating system how can you access it?
(Manager)**
  - You need the appropriate manager for this service (e.g. SensorManager for sensors)
  - You can access service through manager with defined Context.XXX_SERVICE constants
- **Explain (given) source code related to services!**
  - TODO
- **If you want to play an audio file using MediaPlayer that is stored on the web,
which steps do you have to take?**
  - Get the URL of the audio file
  - Download the file
  - Initialize the MediaPlayer with the downloaded file
  - Start playing the audio file

### Location

- **From which sources can a device determine its position?**
  - Satellite Navigation (GPS)
  - Mobile Network (from Cell id)
  - Wifi-networks
- **Signals from how many satellites (GPS) are at least required to determine
position?**
  - 4 satellites are required to determine position
- **Using which numbers can we specify a location on earth?**
  - Latitude, Longitude
- **Which APIs can we use to determine location? Advantages/Disadvantages of
each?**
  - Android Framework Location API
    - Advantages:
      - Easy to use
      - Works on all Android devices
    - Disadvantages:
      - Not very accurate
      - Not very fast
      - Not very power efficient
  - Google Location Services API
    - Advantages:
      - Accurate
      - Fast
      - Power efficient
    - Disadvantages:
      - Requires Google Play Services
      - Requires Google Account
      - Requires Internet connection
- **Which basic steps do you have to take if you want to track your location using
the Google Location Services API? (basic description or explaining given
source code)**
  - Require and check permission
    - ACCESS_COARSE_LOCATION (ungenau) or ACCESS_FINE_LOCATION (genau)
  - Obtain FusedLocationProviderClient
  - Get last known location
  - Slide 11.14


### Alternative approaches

- **What are the advantages/limitations of using alternative development
approaches in general?**
  - Advantages:
    - no additional cost for porting
    - No double code bases
    - Sometimes possible to reuse existing knowledge
  - Disadvantages:
    - Often additional layers of complexity
- **What basic different types of approaches exist? Name one example each!
What are pros/cons?**
  - Web- and Hybrid-Apps
    - Ionic
    - JQuery Mobile
    - Pro
      - TODO
    - Con
      - TODO
  - Native apps
    - React native
    - Nativescript
    - Flutter
    - Pro
      - TODO
    - Con
      - TODO
- **What is a hybrid app?**
  - Web-Hybrid
      - Consists out of a web view inside a wrapper to allow access to device interfaces
      - Extended “WebView”
  - Native-Hybrid
    - Native App with parts developed using HTML and JS
- **Using which language do you develop for Flutter / Ionic Framework?**
  - Iconic: HTML, CSS, JavaScript
  - Flutter: Dart, C(++/#), JavaScript
