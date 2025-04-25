# Module pattern - Page 6
1.	const UserService = (function() {
2.	    // Simulating a heavy initialization logic that is only executed once
3.	    let users = [];
4.	
5.	    // Lazy loading - Initialization of the users list happens only once, on first use
6.	    function initialize() {
7.	        if (users.length === 0) {
8.	            console.log("Initializing users...");
9.	            // Simulated delay for user loading, to showcase lazy loading
10.	            users = [{ id: 1, name: 'Alice' }, { id: 2, name: 'Bob' }];
11.	        }
12.	    }
13.	
14.	    function addUser(user) {
15.	        initialize(); // Ensure initialization happens before use
16.	        users.push(user);
17.	    }
18.	
19.	    function getUser(id) {
20.	        initialize(); // Ensure initialization happens before use
21.	        return users.find(user => user.id === id);
22.	    }
23.	
24.	    return {
25.	        addUser: addUser,
26.	        getUser: getUser
27.	    };
28.	})();


# Revealing Module Pattern - Page 8
1.	const AuthService = (function() {
2.	  let isAuthenticated = false;
3.	
4.	  function login(user) {
5.	    isAuthenticated = true;
6.	    console.log(`${user} logged in`);
7.	  }
8.	
9.	  function logout() {
10.	    isAuthenticated = false;
11.	    console.log('Logged out');
12.	  }
13.	
14.	  function isLoggedIn() {
15.	    return isAuthenticated;
16.	  }
17.	
18.	  return {
19.	    login: login,
20.	    logout: logout,
21.	    isLoggedIn: isLoggedIn
22.	  };
23.	})();
24.	
25.	// Usage
26.	AuthService.login('Alice');
27.	console.log(AuthService.isLoggedIn()); // true
28.	AuthService.logout();
29.	console.log(AuthService.isLoggedIn()); // false


# Singleton pattern - Page 8
1.	const ConfigManager = (function() {
2.	  let instance;
3.	
4.	  function createInstance() {
5.	    const config = {
6.	      apiUrl: 'https://api.example.com',
7.	      timeout: 5000
8.	    };
9.	    return config;
10.	  }
11.	
12.	  return {
13.	    getInstance: function() {
14.	      if (!instance) {
15.	        instance = createInstance();
16.	      }
17.	      return instance;
18.	    }
19.	  };
20.	})();
21.	
22.	// Usage
23.	const config1 = ConfigManager.getInstance();
24.	const config2 = ConfigManager.getInstance();
25.	console.log(config1 === config2); // true


# Observer pattern - Page 9
1.	class EventEmitter {
2.	  constructor() {
3.	    this.events = {};
4.	  }
5.	
6.	  on(event, listener) {
7.	    if (!this.events[event]) {
8.	      this.events[event] = [];
9.	    }
10.	    this.events[event].push(listener);
11.	  }
12.	
13.	  emit(event, data) {
14.	    if (this.events[event]) {
15.	      this.events[event].forEach(listener => listener(data));
16.	    }
17.	  }
18.	}
19.	
20.	const eventEmitter = new EventEmitter();
21.	
22.	function logger(data) {
23.	  console.log('Logging data:', data);
24.	}
25.	
26.	eventEmitter.on('dataReceived', logger);
27.	eventEmitter.emit('dataReceived', { id: 1, message: 'Hello World!' }); // Logging data: { id: 1, message: 'Hello World!' }


# Factory pattern - Page 10
1.	class User {
2.	  constructor(name) {
3.	    this.name = name;
4.	  }
5.	}
6.	
7.	class Admin {
8.	  constructor(name) {
9.	    this.name = name;
10.	  }
11.	}
12.	
13.	class UserFactory {
14.	  createUser(type, name) {
15.	    switch(type) {
16.	      case 'user':
17.	        return new User(name);
18.	      case 'admin':
19.	        return new Admin(name);
20.	      default:
21.	        throw new Error('Unknown user type');
22.	    }
23.	  }
24.	}
25.	
26.	const factory = new UserFactory();
27.	const regularUser = factory.createUser('user', 'Alice');
28.	const adminUser = factory.createUser('admin', 'Bob');
29.	
30.	console.log(regularUser instanceof User); // true
31.	console.log(adminUser instanceof Admin); // true


# Decorator Pattern - Page 11
1.	function Order() {
2.	  this.total = 100;
3.	}
4.	
5.	function addShipping(order) {
6.	  order.total += 10;
7.	}
8.	
9.	function addTax(order) {
10.	  order.total *= 1.15;
11.	}
12.	
13.	const myOrder = new Order();
14.	addShipping(myOrder);
15.	addTax(myOrder);
16.	
17.	console.log(myOrder.total); // 126.5


# Strategy pattern - Page 12
1.	class Payment {
2.	  constructor(strategy) {
3.	    this.strategy = strategy;
4.	  }
5.	
6.	  setStrategy(strategy) {
7.	    this.strategy = strategy;
8.	  }
9.	
10.	  execute(amount) {
11.	    return this.strategy.pay(amount);
12.	  }
13.	}
14.	
15.	const creditCardPayment = {
16.	  pay: (amount) => `Paid ${amount} using credit card`
17.	};
18.	
19.	const paypalPayment = {
20.	  pay: (amount) => `Paid ${amount} using PayPal`
21.	};
22.	
23.	const payment = new Payment(creditCardPayment);
24.	console.log(payment.execute(100)); // Paid 100 using credit card
25.	
26.	payment.setStrategy(paypalPayment);
27.	console.log(payment.execute(200)); // Paid 200 using PayPal


# Proxy pattern - Page 13
1.	class APIService {
2.	  fetchData() {
3.	    return 'Data from API';
4.	  }
5.	}
6.	
7.	class APIServiceProxy {
8.	  constructor() {
9.	    this.apiService = new APIService();
10.	    this.cache = null;
11.	  }
12.	
13.	  fetchData() {
14.	    if (!this.cache) {
15.	      this.cache = this.apiService.fetchData();
16.	    }
17.	    return this.cache;
18.	  }
19.	}
20.	
21.	const proxy = new APIServiceProxy();
22.	console.log(proxy.fetchData()); // Data from API
23.	console.log(proxy.fetchData()); // Data from API (cached)


# Singleton pattern to improve availability - Page 14 + 15
1.	class ConfigurationManager {
2.	  constructor() {
3.	    if (!ConfigurationManager.instance) {
4.	      this.config = {}; // Load configuration settings
5.	      ConfigurationManager.instance = this;
6.	    }
7.	    return ConfigurationManager.instance;
8.	  }
9.	
10.	  get(key) {
11.	    return this.config[key];
12.	  }
13.	
14.	  set(key, value) {
15.	    this.config[key] = value;
16.	  }
17.	}
18.	
19.	const configManager = new ConfigurationManager();
20.	Object.freeze(configManager);
21.	export default configManager;

1.	import configManager from './ConfigurationManager';
2.	
3.	configManager.set('apiUrl', 'https://api.example.com');
4.	console.log(configManager.get('apiUrl'));



# Observer pattern to improve availability - Page 15
1.	class Observer {
2.	  constructor() {
3.	    this.observers = [];
4.	  }
5.	
6.	  subscribe(fn) {
7.	    this.observers.push(fn);
8.	  }
9.	
10.	  unsubscribe(fn) {
11.	    this.observers = this.observers.filter(subscriber => subscriber !== fn);
12.	  }
13.	
14.	  notify(data) {
15.	    this.observers.forEach(subscriber => subscriber(data));
16.	  }
17.	}
18.	
19.	// Usage
20.	const userObserver = new Observer();
21.	
22.	function updateUserInterface(data) {
23.	  console.log(`User Interface Updated: ${data}`);
24.	}
25.	
26.	function logUserChange(data) {
27.	  console.log(`User Change Logged: ${data}`);
28.	}
29.	
30.	userObserver.subscribe(updateUserInterface);
31.	userObserver.subscribe(logUserChange);
32.	
33.	// Simulate user data change
34.	userObserver.notify('User data changed');


# Decorator pattern to improve availability - Page 16
1.	function withCaching(fn) {
2.	  const cache = new Map();
3.	  return async function(...args) {
4.	    const key = JSON.stringify(args);
5.	    if (cache.has(key)) {
6.	      console.log('Returning cached data');
7.	      return cache.get(key);
8.	    }
9.	    const result = await fn(...args);
10.	    cache.set(key, result);
11.	    return result;
12.	  };
13.	}
14.	
15.	async function fetchData(apiEndpoint) {
16.	  const response = await fetch(apiEndpoint);
17.	  return response.json();
18.	}
19.	
20.	const fetchDataWithCaching = withCaching(fetchData);
21.	
22.	// Usage
23.	fetchDataWithCaching('https://api.example.com/data').then(console.log);
24.	fetchDataWithCaching('https://api.example.com/data').then(console.log); // Cached


# Proxy pattern to improve availability - Page 17
1.	class APIService {
2.	  async fetchData(apiEndpoint) {
3.	    const response = await fetch(apiEndpoint);
4.	    if (!response.ok) throw new Error('Network response was not ok');
5.	    return response.json();
6.	  }
7.	}
8.	
9.	class APIServiceProxy {
10.	  constructor() {
11.	    this.apiService = new APIService();
12.	    this.retryLimit = 3;
13.	  }
14.	
15.	  async fetchData(apiEndpoint) {
16.	    let attempts = 0;
17.	    while (attempts < this.retryLimit) {
18.	      try {
19.	        return await this.apiService.fetchData(apiEndpoint);
20.	      } catch (error) {
21.	        attempts += 1;
22.	        console.error(`Attempt ${attempts} failed: ${error.message}`);
23.	        if (attempts >= this.retryLimit) {
24.	          return { error: 'Service unavailable, please try again later.' };
25.	        }
26.	      }
27.	    }
28.	  }
29.	}
30.	
31.	// Usage
32.	const apiServiceProxy = new APIServiceProxy();
33.	apiServiceProxy.fetchData('https://api.example.com/data').then(console.log);


# Factory pattern to improve availability - Page 18
1.	class EmailNotification {
2.	    send(message) {
3.	        console.log(`Sending email: ${message}`);
4.	    }
5.	}
6.	
7.	class SMSNotification {
8.	    send(message) {
9.	        console.log(`Sending SMS: ${message}`);
10.	    }
11.	}
12.	
13.	// Factory class responsible for creating notification objects
14.	class NotificationFactory {
15.	    // Factory method to create notifications based on the type provided
16.	    createNotification(type) {
17.	        switch (type) {
18.	            case 'email':
19.	                return new EmailNotification();
20.	            case 'sms':
21.	                return new SMSNotification();
22.	            // Extensibility: Adding a new notification type is as simple as adding a new case
23.	            case 'push':
24.	                return new PushNotification(); // Example of adding a new type (Push Notification)
25.	            default:
26.	                throw new Error('Invalid notification type');
27.	        }
28.	    }
29.	}
30.	
31.	// New type of notification that can be added easily
32.	class PushNotification {
33.	    send(message) {
34.	        console.log(`Sending push notification: ${message}`);
35.	    }
36.	}
37.	
38.	// Usage
39.	const factory = new NotificationFactory();
40.	const emailNotification = factory.createNotification('email');
41.	const smsNotification = factory.createNotification('sms');
42.	const pushNotification = factory.createNotification('push'); // New type added
43.	
44.	emailNotification.send('Hello via Email!');
45.	smsNotification.send('Hello via SMS!');
46.	pushNotification.send('Hello via Push Notification!');


# Module pattern to improve performance - Page 20
1.	const HeavyModule = (function() {
2.	    let module;
3.	    return function() {
4.	        if (!module) {
5.	            // Perform heavy initialization only when needed
6.	            module = { /* heavy initialization */ };
7.	        }
8.	        return module;
9.	    };
10.	})();


# Singleton pattern to improve performance - Page 21
1.	const Config = (function() {
2.	    let instance;
3.	    return {
4.	        getInstance: function() {
5.	            if (!instance) {
6.	                instance = { /* fetch or compute config */ };
7.	            }
8.	            return instance;
9.	        }
10.	    };
11.	})();


# Observer pattern to improve performance - Page 21
1.	class Store {
2.	    constructor() {
3.	        this.state = {};
4.	        this.observers = [];
5.	    }
6.	
7.	    subscribe(observer) {
8.	        this.observers.push(observer);
9.	    }
10.	
11.	    setState(newState) {
12.	        this.state = { ...this.state, ...newState };
13.	        this.notify();
14.	    }
15.	
16.	    notify() {
17.	        this.observers.forEach(observer => observer.update(this.state));
18.	    }
19.	}


# Factory pattern to improve performance - Page 22
1.	class ObjectPool {
2.	    constructor(createFn) {
3.	        this.createFn = createFn;
4.	        this.pool = [];
5.	    }
6.	
7.	    get() {
8.	        return this.pool.length ? this.pool.pop() : this.createFn();
9.	    }
10.	
11.	    release(obj) {
12.	        this.pool.push(obj);
13.	    }
14.	}


# Decorator pattern to improve performance - Page 23
1.	function cacheDecorator(fn) {
2.	    const cache = new Map();
3.	    return function(...args) {
4.	        const key = JSON.stringify(args);
5.	        if (cache.has(key)) {
6.	            return cache.get(key);
7.	        }
8.	        const result = fn(...args);
9.	        cache.set(key, result);
10.	        return result;
11.	    };
12.	}


# Proxy pattern to improve performance - Page 23
1.	class ImageProxy {
2.	    constructor(filename) {
3.	        this.filename = filename;
4.	        this.image = null;
5.	    }
6.	
7.	    display() {
8.	        if (!this.image) {
9.	            this.image = new RealImage(this.filename);
10.	        }
11.	        this.image.display();
12.	    }
13.	}

