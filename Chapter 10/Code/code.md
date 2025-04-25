# Singleton pattern - Page 4
1.	class ConfigManager {
2.	    constructor() {
3.	        if (!ConfigManager.instance) {
4.	            this.config = {};
5.	            ConfigManager.instance = this;
6.	        }
7.	        return ConfigManager.instance;
8.	    }
9.	
10.	    set(key, value) {
11.	        this.config[key] = value;
12.	    }
13.	
14.	    get(key) {
15.	        return this.config[key];
16.	    }
17.	}
18.	
19.	const instance = new ConfigManager();
20.	Object.freeze(instance);
21.	export default instance;


# Singleton pattern test - Page 5
1.	import ConfigManager from './ConfigManager';
2.	
3.	test('ConfigManager instance should be unique', () => {
4.	    const instance1 = ConfigManager;
5.	    const instance2 = ConfigManager;
6.	    expect(instance1).toBe(instance2);
7.	});
8.	
9.	test('ConfigManager should store and retrieve values', () => {
10.	    const config = ConfigManager;
11.	    config.set('appName', 'MyApp');
12.	    expect(config.get('appName')).toBe('MyApp');
13.	});


# Factory pattern - Page 5
1.	class EmailNotification {
2.	    send(message) {
3.	        return `Email sent: ${message}`;
4.	    }
5.	}
6.	
7.	class SMSNotification {
8.	    send(message) {
9.	        return `SMS sent: ${message}`;
10.	    }
11.	}
12.	
13.	class PushNotification {
14.	    send(message) {
15.	        return `Push notification sent: ${message}`;
16.	    }
17.	}
18.	
19.	class NotificationFactory {
20.	    static createNotification(type) {
21.	        switch(type) {
22.	            case 'email':
23.	                return new EmailNotification();
24.	            case 'sms':
25.	                return new SMSNotification();
26.	            case 'push':
27.	                return new PushNotification();
28.	            default:
29.	                throw new Error('Invalid notification type');
30.	        }
31.	    }
32.	}
33.	
34.	export default NotificationFactory;


# Factory pattern test - Page 6
1.	import NotificationFactory from './NotificationFactory';
2.	import EmailNotification from './EmailNotification';
3.	import SMSNotification from './SMSNotification';
4.	import PushNotification from './PushNotification';
5.	
6.	test('Factory should create an email notification object', () => {
7.	    const notification = NotificationFactory.createNotification('email');
8.	    expect(notification).toBeInstanceOf(EmailNotification);
9.	    expect(notification.send('Hello')).toBe('Email sent: Hello');
10.	});
11.	
12.	test('Factory should create an SMS notification object', () => {
13.	    const notification = NotificationFactory.createNotification('sms');
14.	    expect(notification).toBeInstanceOf(SMSNotification);
15.	    expect(notification.send('Hello')).toBe('SMS sent: Hello');
16.	});
17.	
18.	test('Factory should create a push notification object', () => {
19.	    const notification = NotificationFactory.createNotification('push');
20.	    expect(notification).toBeInstanceOf(PushNotification);
21.	    expect(notification.send('Hello')).toBe('Push notification sent: Hello');
22.	});


# Observer pattern - Page 7
1.	class Newsletter {
2.	    constructor() {
3.	        this.subscribers = [];
4.	    }
5.	
6.	    subscribe(subscriber) {
7.	        this.subscribers.push(subscriber);
8.	    }
9.	
10.	    unsubscribe(subscriber) {
11.	        this.subscribers = this.subscribers.filter(sub => sub !== subscriber);
12.	    }
13.	
14.	    notify(message) {
15.	        this.subscribers.forEach(subscriber => subscriber.update(message));
16.	    }
17.	}
18.	
19.	class Subscriber {
20.	    constructor(name) {
21.	        this.name = name;
22.	    }
23.	
24.	    update(message) {
25.	        return `${this.name} received: ${message}`;
26.	    }
27.	}
28.	
29.	export { Newsletter, Subscriber };


# Observer pattern test - Page 8
1.	import { Newsletter, Subscriber } from './Newsletter';
2.	
3.	test('Subscribers should be notified of new messages', () => {
4.	    const newsletter = new Newsletter();
5.	    const subscriber1 = new Subscriber('Alice');
6.	    const subscriber2 = new Subscriber('Bob');
7.	
8.	    jest.spyOn(subscriber1, 'update');
9.	    jest.spyOn(subscriber2, 'update');
10.	
11.	    newsletter.subscribe(subscriber1);
12.	    newsletter.subscribe(subscriber2);
13.	
14.	    newsletter.notify('New Edition Available');
15.	
16.	    expect(subscriber1.update).toHaveBeenCalledWith('New Edition Available');
17.	    expect(subscriber2.update).toHaveBeenCalledWith('New Edition Available');
18.	});


# Strategy pattern - Page 9
1.	class CreditCardPayment {
2.	    process(amount) {
3.	        return `Processing credit card payment of ${amount}`;
4.	    }
5.	}
6.	
7.	class PayPalPayment {
8.	    process(amount) {
9.	        return `Processing PayPal payment of ${amount}`;
10.	    }
11.	}
12.	
13.	class BitcoinPayment {
14.	    process(amount) {
15.	        return `Processing Bitcoin payment of ${amount}`;
16.	    }
17.	}
18.	
19.	class PaymentContext {
20.	    constructor(strategy) {
21.	        this.strategy = strategy;
22.	    }
23.	
24.	    executeStrategy(amount) {
25.	        return this.strategy.process(amount);
26.	    }
27.	}
28.	
29.	export { PaymentContext, CreditCardPayment, PayPalPayment, BitcoinPayment };


# Strategy pattern test - Page 9
1.	import { PaymentContext, CreditCardPayment, PayPalPayment, BitcoinPayment } from './PaymentStrategy';
2.	
3.	test('Credit card payment strategy should process payment', () => {
4.	    const strategy = new CreditCardPayment();
5.	    const context = new PaymentContext(strategy);
6.	    const result = context.executeStrategy(100);
7.	    expect(result).toBe('Processing credit card payment of 100');
8.	});
9.	
10.	test('PayPal payment strategy should process payment', () => {
11.	    const strategy = new PayPalPayment();
12.	    const context = new PaymentContext(strategy);
13.	    const result = context.executeStrategy(100);
14.	    expect(result).toBe('Processing PayPal payment of 100');
15.	});
16.	
17.	test('Bitcoin payment strategy should process payment', () => {
18.	    const strategy = new BitcoinPayment();
19.	    const context = new PaymentContext(strategy);
20.	    const result = context.executeStrategy(100);
21.	    expect(result).toBe('Processing Bitcoin payment of 100');
22.	});


# Decorator pattern - Page 10
1.	class Coffee {
2.	    cost() {
3.	        return 5;
4.	    }
5.	}
6.	
7.	class MilkDecorator {
8.	    constructor(coffee) {
9.	        // Validate that the coffee object is properly initialized
10.	        if (!coffee || typeof coffee.cost !== 'function') {
11.	            throw new Error('Invalid coffee object provided to MilkDecorator.');
12.	        }
13.	        this.coffee = coffee;
14.	    }
15.	
16.	    cost() {
17.	        return this.coffee.cost() + 1; // Adds the cost of milk
18.	    }
19.	}
20.	
21.	class SugarDecorator {
22.	    constructor(coffee) {
23.	        // Validate that the coffee object is properly initialized
24.	        if (!coffee || typeof coffee.cost !== 'function') {
25.	            throw new Error('Invalid coffee object provided to SugarDecorator.');
26.	        }
27.	        this.coffee = coffee;
28.	    }
29.	
30.	    cost() {
31.	        return this.coffee.cost() + 0.5; // Adds the cost of sugar
32.	    }
33.	}
34.	
35.	export { Coffee, MilkDecorator, SugarDecorator };


# Decorator pattern test - Page 11
1.	import { Coffee, MilkDecorator, SugarDecorator } from './CoffeeDecorator';
2.	
3.	test('Cost of coffee should be 5', () => {
4.	    const coffee = new Coffee();
5.	    expect(coffee.cost()).toBe(5);
6.	});
7.	
8.	test('Cost of coffee with milk should be 6', () => {
9.	    const coffee = new MilkDecorator(new Coffee());
10.	    expect(coffee.cost()).toBe(6);
11.	});
12.	
13.	test('Cost of coffee with milk and sugar should be 6.5', () => {
14.	    const coffee = new SugarDecorator(new MilkDecorator(new Coffee()));
15.	    expect(coffee.cost()).toBe(6.5);
16.	});


# Adapter pattern - Page 12
1.	class OldLogger {
2.	    log(message) {
3.	        return `Old Logger: ${message}`;
4.	    }
5.	}
6.	
7.	class NewLogger {
8.	    writeLog(message) {
9.	        return `New Logger: ${message}`;
10.	    }
11.	}
12.	
13.	class LoggerAdapter {
14.	    constructor(logger) {
15.	        this.logger = logger;
16.	    }
17.	
18.	    log(message) {
19.	        return this.logger.writeLog(message);
20.	    }
21.	}
22.	
23.	export { OldLogger, NewLogger, LoggerAdapter };


# Adapter pattern test - Page 12
1.	import { OldLogger, NewLogger, LoggerAdapter } from './LoggerAdapter';
2.	
3.	test('OldLogger should log messages correctly', () => {
4.	    const oldLogger = new OldLogger();
5.	    expect(oldLogger.log('Test message')).toBe('Old Logger: Test message');
6.	});
7.	
8.	test('LoggerAdapter should adapt NewLogger to OldLogger interface', () => {
9.	    const newLogger = new NewLogger();
10.	    const adapter = new LoggerAdapter(newLogger);
11.	    expect(adapter.log('Test message')).toBe('New Logger: Test message');
12.	});


# Builder pattern - Page 13
1.	class Meal {
2.	    constructor() {
3.	        this.parts = [];
4.	    }
5.	
6.	    addPart(part) {
7.	        this.parts.push(part);
8.	    }
9.	
10.	    showMeal() {
11.	        return `Meal contains: ${this.parts.join(', ')}`;
12.	    }
13.	}
14.	
15.	class MealBuilder {
16.	    constructor() {
17.	        this.meal = new Meal();
18.	    }
19.	
20.	    addBurger() {
21.	        this.meal.addPart('Burger');
22.	        return this;
23.	    }
24.	
25.	    addDrink() {
26.	        this.meal.addPart('Drink');
27.	        return this;
28.	    }
29.	
30.	    addFries() {
31.	        this.meal.addPart('Fries');
32.	        return this;
33.	    }
34.	
35.	    build() {
36.	        return this.meal;
37.	    }
38.	}
39.	
40.	export { Meal, MealBuilder };


# Builder pattern test - page 14
1.	import { MealBuilder } from './MealBuilder';
2.	
3.	test('MealBuilder should create a meal with a burger, drink, and fries', () => {
4.	    const builder = new MealBuilder();
5.	    const meal = builder.addBurger().addDrink().addFries().build();
6.	    expect(meal.showMeal()).toBe('Meal contains: Burger, Drink, Fries');
7.	});
8.	
9.	test('MealBuilder should create a meal with only a burger and drink', () => {
10.	    const builder = new MealBuilder();
11.	    const meal = builder.addBurger().addDrink().build();
12.	    expect(meal.showMeal()).toBe('Meal contains: Burger, Drink');
13.	});


# Prototype pattern - Page 14
1.	class UserProfile {
2.	    constructor(name, age, email) {
3.	        this.name = name;
4.	        this.age = age;
5.	        this.email = email;
6.	    }
7.	
8.	    clone() {
9.	        return new UserProfile(this.name, this.age, this.email);
10.	    }
11.	}
12.	
13.	export default UserProfile;


# Prototype pattern test - Page 15
1.	import UserProfile from './UserProfile';
2.	
3.	test('UserProfile should be cloned correctly', () => {
4.	    const originalProfile = new UserProfile('John Doe', 30, 'john.doe@example.com');
5.	    const clonedProfile = originalProfile.clone();
6.	
7.	    expect(clonedProfile).not.toBe(originalProfile); // Ensure it's a new object
8.	    expect(clonedProfile.name).toBe('John Doe');
9.	    expect(clonedProfile.age).toBe(30);
10.	    expect(clonedProfile.email).toBe('john.doe@example.com');
11.	});


# Mediator pattern - Page 15
1.	class ChatRoom {
2.	    showMessage(user, message) {
3.	        const time = new Date().toLocaleTimeString();
4.	        return `${time} [${user}]: ${message}`;
5.	    }
6.	}
7.	
8.	class User {
9.	    constructor(name, chatRoom) {
10.	        this.name = name;
11.	        this.chatRoom = chatRoom;
12.	    }
13.	
14.	    send(message) {
15.	        return this.chatRoom.showMessage(this.name, message);
16.	    }
17.	}
18.	
19.	export { ChatRoom, User };


# Mediator pattern test - Page 16
1.	import { ChatRoom, User } from './ChatRoom';
2.	
3.	test('ChatRoom should mediate messages between users', () => {
4.	    const chatRoom = new ChatRoom();
5.	    const user1 = new User('Alice', chatRoom);
6.	    const user2 = new User('Bob', chatRoom);
7.	
8.	    const message1 = user1.send('Hello, Bob!');
9.	    const message2 = user2.send('Hi, Alice!');
10.	
11.	    expect(message1).toMatch(/^\d{1,2}:\d{2}:\d{2} [APM]{2} \[Alice\]: Hello, Bob!$/);
12.	    expect(message2).toMatch(/^\d{1,2}:\d{2}:\d{2} [APM]{2} \[Bob\]: Hi, Alice!$/);
13.	});


# Chain of responsibility pattern - Page 17
•	class SupportHandler {
•	    setNext(handler) {
•	        this.nextHandler = handler;
•	        return handler;
•	    }
•	
•	    handle(request) {
•	        if (this.nextHandler) {
•	            return this.nextHandler.handle(request);
•	        }
•	        return null;
•	    }
•	}
•	
•	class LowLevelSupport extends SupportHandler {
•	    handle(request) {
•	        if (request.level === 'low') {
•	            return `Low-level support handling ticket: ${request.message}`;
•	        }
•	        return super.handle(request);
•	    }
•	}
•	
•	class MidLevelSupport extends SupportHandler {
•	    handle(request) {
•	        if (request.level === 'mid') {
•	            return `Mid-level support handling ticket: ${request.message}`;
•	        }
•	        return super.handle(request);
•	    }
•	}
•	
•	class HighLevelSupport extends SupportHandler {
•	    handle(request) {
•	        if (request.level === 'high') {
•	            return `High-level support handling ticket: ${request.message}`;
•	        }
•	        return super.handle(request);
•	    }
•	}
•	
•	export { LowLevelSupport, MidLevelSupport, HighLevelSupport };


# Chain of responsibility pattern test - Page 18
1.	import { LowLevelSupport, MidLevelSupport, HighLevelSupport } from './SupportHandlers';
2.	
3.	test('Low-level support should handle low-level requests', () => {
4.	    const lowLevelSupport = new LowLevelSupport();
5.	    const midLevelSupport = new MidLevelSupport();
6.	    const highLevelSupport = new HighLevelSupport();
7.	
8.	    lowLevelSupport.setNext(midLevelSupport).setNext(highLevelSupport);
9.	
10.	    const request = { level: 'low', message: 'Reset password' };
11.	    expect(lowLevelSupport.handle(request)).toBe('Low-level support handling ticket: Reset password');
12.	});
13.	
14.	test('Mid-level support should handle mid-level requests', () => {
15.	    const lowLevelSupport = new LowLevelSupport();
16.	    const midLevelSupport = new MidLevelSupport();
17.	    const highLevelSupport = new HighLevelSupport();
18.	
19.	    lowLevelSupport.setNext(midLevelSupport).setNext(highLevelSupport);
20.	
21.	    const request = { level: 'mid', message: 'System outage' };
22.	    expect(midLevelSupport.handle(request)).toBe('Mid-level support handling ticket: System outage');
23.	});
24.	
25.	test('High-level support should handle high-level requests', () => {
26.	    const lowLevelSupport = new LowLevelSupport();
27.	    const midLevelSupport = new MidLevelSupport();
28.	    const highLevelSupport = new HighLevelSupport();
29.	
30.	    lowLevelSupport.setNext(midLevelSupport).setNext(highLevelSupport);
31.	
32.	    const request = { level: 'high', message: 'Security breach' };
33.	    expect(highLevelSupport.handle(request)).toBe('High-level support handling ticket: Security breach');
34.	});


