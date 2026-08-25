### Observer Design Pattern

 - Suppose there is a class called Stock, and there are other classes which are required to be notified upon any change in the stock prices.
 - Observer Pattern is fundamentally about decoupling the publisher from its consumer.
 - Rather than hardcoding the calls to notify the consumer, Observer pattern suggest to keep a list of common interface implemented by each of the consumer.
 - And calls notify/update method whenever there is a change in the publisher class in loop wise manner.
 - An event Listener is often essentially an implementation of the observer idea. The listener is the observer.
 
##### Structure

1. Interface
```java
	interface Observer {
		void update();
	}
```

2. Subject
```java
	interface Subject {
		void subscribe(Observer observer);
		
		void unsubscribe(Observer observer);
		
		void notifyObservers();
	}
```

3. Concrete Subject
```java
	class Stock implements Subject {

		private double price;

		private final List<Observer> observers = new ArrayList<>();

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
				observer.update();
			}
		}

		public void setPrice(double price) {
			this.price = price;
			notifyObservers();
		}

		public double getPrice() {
			return price;
		}
	}
```

#### Push vs Pull Model

###### Pull Model
 - Observer gets notified:
```java
	observer.update();
```
 - Then observer asks the Subject:
```java
	stock.getPrice();
```

 - Advantage : Observer decides what information it needs.
 - Disadvantage : Observer becomes coupled to the Subject's API.

###### Push Model
 - Subject sends the changed information directly.
```java
	interface Observer {
		void update(double price);
	}
```
```java
Subject:

@Override
public void notifyObservers() {
    for (Observer observer : observers) {
        observer.update(price);
    }
}
```

Advantage - Simple and efficient.
Disadvantage - Subject must decide what data observers receive

##### Observer vs Pub/Sub
 - Observer is primarily an object-level behavioral pattern. Pub/Sub is generally a messaging architecture.
 - Observer pattern let's the subject class maintain a list of common interface implemented by each consumer.
 - Observer pattern implementation is often in-process, synchronous, direct method calls, tight lifecycle relationship.
 - In Pub/Sub Pattern, the Publisher doesn't know the consumer. E.g. Kakfa, RabbitMQ, SNS/SQS
 - Pub/Sub can be asynchronous, distributed, persistent, fault-tolerant and independently scalable.

##### Problems with Observer Patterns
###### 1. Order of Notification
 - If we have a specific order in which observers have to be notified, then it must be designed properly for, rather than relying on any generic Data Structure like ArrayList, otherwise some issue may creep in.
###### 2. One slow observer blocks everyone
 - If a call of notification to a observer is slow, it can keep waiting the next observers.
 - The solution to this is use a ExecutorService that can concurrently notify all the observers.
###### 3. Observer throws Exception
 - Exceptions must be caught and handled properly while looping over the observers.
###### 4. Concurrent Modification
 - If an observer unsubscribe itself during notification, we can get `ConcurrentModificationException` or any other surprising behaviour based on collection and concurrency model.
 - A common solution is to iterate over a snapshot:
 ```java
	for (Observer observer: List.copyOf(observers)) {
		observer.update();
	}
 ```
###### 5. Memory Leaks
 - What if a reference to an existing object in the observers list is no longer being used.
 - Subscription management is also lifecycle management, and therefore before being obsolote, the observer object must call unsubsrcibe method to the subject notification.
 
###### 6. Re-entrant Notification 
 - Some designs can be flawed, such that a notification to an Observer can trigger another change that re-triggers another notification.
 
##### Observer and SOLID
 - Observer Supports SRP, OCP, DIP.