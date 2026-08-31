👀 Observer Design Pattern

    The Observer Pattern is a behavioral design pattern where:

    When one object changes, multiple dependent objects are automatically notified.
    Observer Design Pattern is a behavioral pattern that creates a one-to-many relationship between a subject and its observers. When the subject's state changes, all dependent observers are notified and updated automatically, ensuring asynchronized communication.

    Enables automatic updates to multiple objects when one object changes, useful for event-driven or publish-subscribe systems.
    Promotes loose coupling between the subject and its observers, improving flexibility and maintainability.

Simple real-world example 📺

Think about a YouTube channel:

        A YouTube channel uploads a video.
        All subscribers get notified.

Here:

        YouTube Channel → Subject / Publisher
        Subscribers → Observers
        Notification → Update



Problems without the Observer Pattern

Let's use the Weather Station example.

    Suppose temperature changes, and multiple components need that information:

        Mobile App
        TV Display
        Website
        Smartwatch

❌ Without Observer Pattern

    class WeatherStation {
    
        private MobileDisplay mobileDisplay;
        private TVDisplay tvDisplay;
        private WebsiteDisplay websiteDisplay;
    
        public WeatherStation(
                MobileDisplay mobileDisplay,
                TVDisplay tvDisplay,
                WebsiteDisplay websiteDisplay) {
    
            this.mobileDisplay = mobileDisplay;
            this.tvDisplay = tvDisplay;
            this.websiteDisplay = websiteDisplay;
        }
    
        public void setTemperature(double temperature) {
    
            mobileDisplay.update(temperature);
            tvDisplay.update(temperature);
            websiteDisplay.update(temperature);
        }
    }

🚨 Problem 1: Tight Coupling

WeatherStation directly knows about:

    MobileDisplay
    TVDisplay
    WebsiteDisplay

So:

    WeatherStation → MobileDisplay
    WeatherStation → TVDisplay
    WeatherStation → WebsiteDisplay

The WeatherStation is tightly coupled to concrete classes.

🚨 Problem 2: Adding a new consumer requires modification

Tomorrow you add:

    SmartWatchDisplay

You must modify WeatherStation:

smartWatchDisplay.update(temperature);

    So every time a new component wants updates:

❌ Modify the existing publisher.

    This violates the Open/Closed Principle.

🚨 Problem 3: Removing a consumer requires modification

    Suppose the TV display is no longer needed.

Again, you modify:

    WeatherStation

    So both adding and removing consumers affects the publisher.

🚨 Problem 4: Publisher has too many responsibilities

The WeatherStation should only do this:

    Get weather data

    But now it also has to manage:
    
    Who needs updates?
    How do I update them?
    Which classes exist?

Too many responsibilities → violates Single Responsibility Principle.

🚨 Problem 5: Difficult to make it dynamic

Suppose users can subscribe/unsubscribe dynamically:

    User A subscribes ✅
    User B subscribes ✅
    User A unsubscribes ❌
    User C subscribes ✅

Without Observer Pattern, managing this becomes messy.

You may start writing:

    if (mobileEnabled) {
    mobileDisplay.update();
    }
    
    if (tvEnabled) {
    tvDisplay.update();
    }
    
    if (websiteEnabled) {
    websiteDisplay.update();
    }

This becomes harder to maintain.



✅ With Observer Pattern

Now the relationship becomes:

                         WeatherStation
                               |
                         List<Observer>
                               |
                  ┌────────────┼────────────┐
                  ↓            ↓            ↓
              Mobile          TV         Website

WeatherStation only knows:

    List<Observer> observers;

It doesn't care whether the observer is:

    MobileDisplay
    TVDisplay
    Website
    Smartwatch
    EmailService

It simply does:

    for (Observer observer : observers) {
    observer.update(temperature);
    }


1️⃣ Create the Observer interface

    Every object that wants notifications implements this interface.

    interface Observer {
    void update(double temperature);
    }

So every observer must have an update() method.

2️⃣ Create the Subject interface

The Subject manages observers.

    interface Subject {
    
        void subscribe(Observer observer);
    
        void unsubscribe(Observer observer);
    
        void notifyObservers();
    }

It provides three operations:

    subscribe()     → add observer
    unsubscribe()   → remove observer
    notifyObservers() → notify everyone

3️⃣ Create the Publisher / Subject
import java.util.ArrayList;
import java.util.List;

    class WeatherStation implements Subject {
    
        private List<Observer> observers = new ArrayList<>();
    
        private double temperature;
    
        @Override
        public void subscribe(Observer observer) {
            observers.add(observer);
        }
    
        @Override
        public void unsubscribe(Observer observer) {
            observers.remove(observer);
        }
    
        @Override
        public void notifyObservers() {
    
            for (Observer observer : observers) {
                observer.update(temperature);
            }
        }
    
        public void setTemperature(double temperature) {
    
            this.temperature = temperature;
    
            // Temperature changed → notify everyone
            notifyObservers();
        }
    }
🔥 Important part
    
    for (Observer observer : observers) {
    observer.update(temperature);
    }

This is the heart of the Observer Pattern.

The WeatherStation doesn't know whether the observer is:

    Mobile Display
    TV Display
    Website
    Smartwatch

It only knows:

Observer

4️⃣ Create concrete Observers
    
    Mobile Display
    class MobileDisplay implements Observer {
    
        @Override
        public void update(double temperature) {
            System.out.println(
                "Mobile Display: Temperature = " + temperature
            );
        }
    }
TV Display
    
    class TVDisplay implements Observer {
    
        @Override
        public void update(double temperature) {
            System.out.println(
                "TV Display: Temperature = " + temperature
            );
        }
    }
Smartwatch Display
    
    class SmartWatchDisplay implements Observer {
    
        @Override
        public void update(double temperature) {
            System.out.println(
                "Smartwatch: Temperature = " + temperature
            );
        }
    }
5️⃣ Use everything
    
    public class Main {
    
        public static void main(String[] args) {
    
            // Publisher
            WeatherStation weatherStation = new WeatherStation();
    
            // Observers
            Observer mobile = new MobileDisplay();
            Observer tv = new TVDisplay();
            Observer smartwatch = new SmartWatchDisplay();
    
            // Subscribe observers
            weatherStation.subscribe(mobile);
            weatherStation.subscribe(tv);
            weatherStation.subscribe(smartwatch);
    
            // State changes
            System.out.println("Temperature changed!");
    
            weatherStation.setTemperature(30.5);
        }
    }
🔄 What happens internally?
1. Mobile subscribes
2. TV subscribes
3. Smartwatch subscribes

        ↓

WeatherStation temperature changes

        ↓

notifyObservers()

        ↓

observer.update(30.5)

        ↓

Mobile gets update
TV gets update
Smartwatch gets update
🧠 Full relationship
        
        Subject
        ▲
        │ implements
        │
        WeatherStation
        │
        │ has
        ▼
        List<Observer>
        │
        ┌────────────┼─────────────┐
        ▼            ▼             ▼
        Mobile          TV        SmartWatch
        │            │             │
        └────────────┴─────────────┘
        ▲
        │ implements
        Observer


Spring Events are a very good real-world example of the Observer Pattern


    Observer is a behavioral design pattern that establishes a one-to-many relationship between a Subject and multiple Observers. When the state of the Subject changes, it automatically notifies all registered Observers.
    
    For example, consider a WeatherStation. It has multiple consumers such as a mobile display, TV display, and smartwatch. Instead of the WeatherStation directly depending on each concrete display, we define an Observer interface.
    
    Each display implements that interface, and the WeatherStation maintains a list of Observer objects. When the temperature changes, the WeatherStation notifies all observers.
    
    This reduces coupling because the WeatherStation doesn't need to know about MobileDisplay, TVDisplay, or SmartWatchDisplay. New observers can be added without modifying the WeatherStation.
    
    The typical operations are subscribe, unsubscribe, and notifyObservers.