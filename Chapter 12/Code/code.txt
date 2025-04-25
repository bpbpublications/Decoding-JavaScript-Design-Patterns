# Implicit type conversion - Page 2
1.	let a = 20;
2.	a = "Hello World";

1.	let result = 5 + "5";


# Observer pattern problematic code - Page 3
1.	class Subject {
2.	  constructor() {
3.	    this.state = 0;
4.	    this.observers = [];
5.	  }
6.	
7.	  addObserver(observer) {
8.	    this.observers.push(observer);
9.	  }
10.	
11.	  setState(newState) {
12.	    if (newState == this.state) { // Loose equality check
13.	      console.log("State unchanged, no need to notify observers.");
14.	    } else {
15.	      this.state = newState;
16.	      this.notifyObservers();
17.	    }
18.	  }
19.	
20.	  notifyObservers() {
21.	    this.observers.forEach(observer => observer.update(this.state));
22.	  }
23.	}
24.	
25.	class Observer {
26.	  update(state) {
27.	    console.log(`Observer notified, new state: ${state}`);
28.	  }
29.	}
30.	
31.	const subject = new Subject();
32.	const observer1 = new Observer();
33.	subject.addObserver(observer1);
34.	
35.	subject.setState("0"); // Implicit type coercion happens, treated as equal to 0


# Observer pattern solution - Page 4
1.	setState(newState) {
2.	  if (newState === this.state) { // Strict equality check
3.	    console.log("State unchanged, no need to notify observers.");
4.	  } else {
5.	    this.state = newState;
6.	    this.notifyObservers();
7.	  }
8.	}


# Singleton pattern - Page 5
1.	class Singleton {
2.	  constructor() {
3.	    // Check if an instance already exists
4.	    if (Singleton.instance) {
5.	      return Singleton.instance; // Return the existing instance
6.	    }
7.	    
8.	    // If no instance exists, create and store the new instance
9.	    Singleton.instance = this;
10.	    
11.	    this.data = "I am the Singleton instance";
12.	  }
13.	}
14.	
15.	// Usage
16.	const instance1 = new Singleton();
17.	const instance2 = new Singleton();
18.	
19.	console.log(instance1 === instance2); // true, both references point to the same instance
20.	console.log(instance1.data); // Output: "I am the Singleton instance"


# Singleton pattern solution - Page 5
1.	if (Singleton.instance) {
2.	  return Singleton.instance; // Prevents coercion errors
3.	}


# Chain of responsibility problem - Page 6
1.	class Handler {
2.	  constructor(successor = null) {
3.	    this.successor = successor;
4.	  }
5.	
6.	  handle(request) {
7.	    if (!request) { // Implicit coercion, could stop the chain
8.	      console.log("Request is falsy, stopping chain.");
9.	    } else if (this.successor) {
10.	      this.successor.handle(request);
11.	    }
12.	  }
13.	}
14.	
15.	const handler1 = new Handler();
16.	const handler2 = new Handler(handler1);
17.	
18.	handler2.handle(0); // Falsy value, breaks chain even though we might want it to proceed


# Chain of responsibility solution - Page 6
1.	class Handler {
2.	  constructor(successor = null) {
3.	    this.successor = successor;
4.	  }
5.	
6.	  handle(request) {
7.	    if (request === null || request === undefined) { // Explicit checks for stopping
8.	      console.log("Request is invalid, stopping chain.");
9.	    } else if (this.successor) {
10.	      this.successor.handle(request);
11.	    }
12.	  }
13.	}
14.	
15.	const handler1 = new Handler();
16.	const handler2 = new Handler(handler1);
17.	
18.	handler2.handle(0); // Now continues through the chain as intended


# Accidental global problem - Page 8
1.	const myModule = (function () {
2.	  var privateVariable = "I'm private";
3.	
4.	  function publicMethod() {
5.	    undeclaredVariable = "This will be global!";
6.	    console.log(privateVariable);
7.	  }
8.	
9.	  return {
10.	    publicMethod: publicMethod,
11.	  };
12.	})();
13.	
14.	myModule.publicMethod();
15.	console.log(undeclaredVariable); // Outputs: "This will be global!" – Breaks encapsulation


# Accidental global problem solution - Page 9
1.	const myModule = (function () {
2.	  var privateVariable = "I'm private";
3.	
4.	  function publicMethod() {
5.	    let localVariable = "This is local!";
6.	    console.log(privateVariable);
7.	  }
8.	
9.	  return {
10.	    publicMethod: publicMethod,
11.	  };
12.	})();
13.	
14.	myModule.publicMethod();
15.	console.log(typeof localVariable); // Outputs: "undefined" – Correct encapsulation


# Unexpected behaviour due to hoisting problem - Page 9
1.	function factory() {
2.	  console.log(product); // Outputs: undefined due to hoisting
3.	  var product = createProduct(); // Variable is hoisted, but assignment happens later
4.	}
5.	
6.	function createProduct() {
7.	  return { name: "New Product" };
8.	}
9.	
10.	factory();


# Unexpected behaviour due to hoisting solution - Page 10
1.	function factory() {
2.	  var product = createProduct(); // Declare and assign before usage
3.	  console.log(product); // Outputs: { name: "New Product" }
4.	}
5.	
6.	function createProduct() {
7.	  return { name: "New Product" };
8.	}
9.	
10.	factory();


# Scope binding in callbacks problem - Page 10
1.	class Observer {
2.	  constructor(name) {
3.	    this.name = name;
4.	  }
5.	
6.	  update() {
7.	    setTimeout(function () {
8.	      console.log(this.name); // `this` refers to the global object, not the observer
9.	    }, 1000);
10.	  }
11.	}
12.	
13.	const observer = new Observer("Observer 1");
14.	observer.update(); // Outputs: undefined


# Scope binding in callbacks solution - Page 11
1.	class Observer {
2.	  constructor(name) {
3.	    this.name = name;
4.	  }
5.	
6.	  update() {
7.	    setTimeout(() => { // Arrow function maintains the lexical scope of `this`
8.	      console.log(this.name); // Correctly refers to the Observer instance
9.	    }, 1000);
10.	  }
11.	}
12.	
13.	const observer = new Observer("Observer 1");
14.	observer.update(); // Outputs: Observer 1


# The "this" keyword misconception - Page 12+13
1.	console.log(this); // In a browser, outputs: Window object

1.	function showThis() {
2.	    console.log(this);
3.	}
4.	showThis(); // In a browser, outputs: Window object

1.	const obj = {
2.	    name: "Alice",
3.	    greet() {
4.	        console.log(`Hello, ${this.name}`);
5.	    }
6.	};
7.	
8.	obj.greet(); // Outputs: "Hello, Alice"

1.	function Person(name) {
2.	    this.name = name;
3.	}
4.	
5.	const person = new Person("Bob");
6.	console.log(person.name); // Outputs: "Bob"

1.	const obj2 = {
2.	    name: "Charlie",
3.	    greet: () => {
4.	        console.log(`Hello, ${this.name}`); // `this` is not bound to `obj2`
5.	    }
6.	};
7.	
8.	obj2.greet(); // Outputs: "Hello, undefined" (if not in strict mode)


1.	const button = document.createElement("button");
2.	button.innerText = "Click me";
3.	button.onclick = function() {
4.	    console.log(this); // Refers to the button element
5.	};
6.	document.body.appendChild(button);


1.	function greet() {
2.	    console.log(`Hello, ${this.name}`);
3.	}
4.	
5.	const user = { name: "David" };
6.	
7.	greet.call(user); // Outputs: "Hello, David"
8.	greet.apply(user); // Outputs: "Hello, David"
9.	
10.	const greetUser = greet.bind(user);
11.	greetUser(); // Outputs: "Hello, David"

1.	"use strict";
2.	
3.	function showStrictThis() {
4.	    console.log(this);
5.	}
6.	
7.	showStrictThis(); // Outputs: undefined


# Misunderstanding the dynamic nature of this (Problem) - Page 14
1.	const myModule = {
2.	  name: "My Module",
3.	  logName: function () {
4.	    console.log(this.name); // Expected to refer to "My Module"
5.	  }
6.	};
7.	
8.	// Direct method call works as expected
9.	myModule.logName(); // Outputs: "My Module"
10.	
11.	// But storing the method in a variable changes the context of `this`
12.	const logNameFunction = myModule.logName;
13.	logNameFunction(); // Outputs: undefined, as `this` now refers to the global object


# Misunderstanding the dynamic nature of this (Solution) - Page 14
1.	const myModule = {
2.	  name: "My Module",
3.	  logName: function () {
4.	    console.log(this.name);
5.	  }
6.	};
7.	
8.	// Use .bind() to explicitly bind the correct `this`
9.	const logNameFunction = myModule.logName.bind(myModule);
10.	logNameFunction(); // Outputs: "My Module"


# Callback Hell - Page 15
1.	function getUser(userId, callback) {
2.	  setTimeout(() => {
3.	    console.log("User fetched");
4.	    callback({ id: userId, name: "John Doe" });
5.	  }, 1000);
6.	}
7.	
8.	function getProfile(user, callback) {
9.	  setTimeout(() => {
10.	    console.log("Profile fetched");
11.	    callback({ ...user, profile: "Developer" });
12.	  }, 1000);
13.	}
14.	
15.	function getPosts(profile, callback) {
16.	  setTimeout(() => {
17.	    console.log("Posts fetched");
18.	    callback({ ...profile, posts: ["Post 1", "Post 2"] });
19.	  }, 1000);
20.	}
21.	
22.	getUser(1, (user) => {
23.	  getProfile(user, (profile) => {
24.	    getPosts(profile, (posts) => {
25.	      console.log(posts);
26.	    });
27.	  });
28.	});
29.	
30.	// Output:
31.	// User fetched
32.	// Profile fetched
33.	// Posts fetched
34.	// { id: 1, name: 'John Doe', profile: 'Developer', posts: [ 'Post 1', 'Post 2' ] }


# Solution to Callback Hell using promises - Page 16
1.	function getUser(userId) {
2.	  return new Promise((resolve) => {
3.	    setTimeout(() => {
4.	      console.log("User fetched");
5.	      resolve({ id: userId, name: "John Doe" });
6.	    }, 1000);
7.	  });
8.	}
9.	
10.	function getProfile(user) {
11.	  return new Promise((resolve) => {
12.	    setTimeout(() => {
13.	      console.log("Profile fetched");
14.	      resolve({ ...user, profile: "Developer" });
15.	    }, 1000);
16.	  });
17.	}
18.	
19.	function getPosts(profile) {
20.	  return new Promise((resolve) => {
21.	    setTimeout(() => {
22.	      console.log("Posts fetched");
23.	      resolve({ ...profile, posts: ["Post 1", "Post 2"] });
24.	    }, 1000);
25.	  });
26.	}
27.	
28.	getUser(1)
29.	  .then(getProfile)
30.	  .then(getPosts)
31.	  .then((posts) => console.log(posts));
32.	
33.	// Output:
34.	// User fetched
35.	// Profile fetched
36.	// Posts fetched
37.	// { id: 1, name: 'John Doe', profile: 'Developer', posts: [ 'Post 1', 'Post 2' ] }


# Solution to Callback Hell using async/await - Page 17
1.	async function fetchUserData() {
2.	  const user = await getUser(1);
3.	  const profile = await getProfile(user);
4.	  const posts = await getPosts(profile);
5.	  console.log(posts);
6.	}
7.	
8.	fetchUserData();
9.	
10.	// Output:
11.	// User fetched
12.	// Profile fetched
13.	// Posts fetched
14.	// { id: 1, name: 'John Doe', profile: 'Developer', posts: [ 'Post 1', 'Post 2' ] }


# Memory leak in Observer pattern - Page 18
1.	function Observer() {
2.	  this.handlers = []; // Observers (event handlers)
3.	}
4.	
5.	Observer.prototype.subscribe = function (fn) {
6.	  this.handlers.push(fn); // Add observer
7.	};
8.	
9.	Observer.prototype.unsubscribe = function (fn) {
10.	  this.handlers = this.handlers.filter(handler => handler !== fn); // Remove observer
11.	};
12.	
13.	Observer.prototype.notify = function (message) {
14.	  this.handlers.forEach(fn => fn(message)); // Notify all observers
15.	};
16.	
17.	const observer = new Observer();
18.	
19.	function logMessage(msg) {
20.	  console.log(msg);
21.	}
22.	
23.	observer.subscribe(logMessage);
24.	
25.	// Forgetting to unsubscribe when done
26.	// Memory leak occurs if observers persist unnecessarily


# Solution for the memory leak in Observer pattern - Page 19
1.	observer.unsubscribe(logMessage);


# Problematic code showing performance bottleneck due to DOM manipulation - Page 19
1.	function updateList(items) {
2.	  const listElement = document.getElementById("item-list");
3.	  
4.	  // Inefficient: modifying the DOM in every loop iteration
5.	  items.forEach(item => {
6.	    const li = document.createElement("li");
7.	    li.textContent = item;
8.	    listElement.appendChild(li); // Causes multiple reflows
9.	  });
10.	}


# Solving performance bottlenecks using Document Fragments - Page 19
1.	function updateList(items) {
2.	  const listElement = document.getElementById("item-list");
3.	  const fragment = document.createDocumentFragment(); // Create a fragment
4.	  
5.	  items.forEach(item => {
6.	    const li = document.createElement("li");
7.	    li.textContent = item;
8.	    fragment.appendChild(li); // Add to fragment, not the DOM
9.	  });
10.	  
11.	  listElement.appendChild(fragment); // Add all at once
12.	}


# Problematic code showing inefficient data handling - Page 20
1.	function Component(name) {
2.	  this.name = name;
3.	  this.children = [];
4.	}
5.	
6.	Component.prototype.add = function (child) {
7.	  this.children.push(child);
8.	};
9.	
10.	Component.prototype.getChildren = function () {
11.	  return this.children;
12.	};
13.	
14.	Component.prototype.traverse = function () {
15.	  console.log(this.name);
16.	  this.children.forEach(child => {
17.	    child.traverse(); // Recursive call
18.	  });
19.	};


# Solving inefficient data handling using memoization - Page 21
1.	Component.prototype.traverse = function () {
2.	  const stack = [this];
3.	  
4.	  while (stack.length > 0) {
5.	    const node = stack.pop();
6.	    console.log(node.name);
7.	    stack.push(...node.children); // Add children to stack
8.	  }
9.	};


# Inefficient DOM manipulation caused by excessive style changes - Page 21
1.	const element = document.getElementById("box");
2.	
3.	// Changing styles one at a time causes multiple reflows
4.	element.style.width = "200px";
5.	element.style.height = "200px";
6.	element.style.backgroundColor = "blue";
7.	element.style.border = "1px solid black";


# Solving inefficient DOM manipulation using CSSOM - Page 22
1.	// Solution 1: Use CSS classes
2.	element.classList.add("new-style"); // Add a class that defines all styles
3.	
4.	// Solution 2: Batch styles in a single operation
5.	element.style.cssText = `
6.	  width: 200px;
7.	  height: 200px;
8.	  background-color: blue;
9.	  border: 1px solid black;
10.	`;


# Problematic code showing unnecessary DOM reads and writes - Page 22
1.	const element = document.getElementById("box");
2.	element.style.width = "200px"; // Write to the DOM
3.	console.log(element.offsetWidth); // Read from the DOM (forces reflow)
4.	element.style.height = "200px"; // Write to the DOM again


# Solution for unnecessary DOM reads and writes - Page 22
1.	const element = document.getElementById("box");
2.	
3.	// Group DOM reads together
4.	const width = element.offsetWidth;
5.	
6.	// Group DOM writes together
7.	element.style.width = "200px";
8.	element.style.height = "200px";


# Problematic code showing direct DOM access in large applications - Page 23
1.	function updateList(items) {
2.	  const listElement = document.getElementById("item-list");
3.	  items.forEach(item => {
4.	    const li = document.createElement("li");
5.	    li.textContent = item;
6.	    listElement.appendChild(li);
7.	  });
8.	}


# Solution for direct DOM access in large applications using a virutal DOM - Page 23
1.	// React Example
2.	function ItemList({ items }) {
3.	  return (
4.	    <ul>
5.	      {items.map(item => (
6.	        <li key={item}>{item}</li>
7.	      ))}
8.	    </ul>
9.	  );
10.	}


# Problematic code showing a lack of proper error handling - Page 24
1.	class DataFetcher {
2.	  constructor(url) {
3.	    this.url = url;
4.	  }
5.	
6.	  fetchData() {
7.	    fetch(this.url)
8.	      .then(response => response.json())
9.	      .then(data => {
10.	        console.log('Data fetched:', data);
11.	      })
12.	      .catch(error => {
13.	        // No proper error handling
14.	        console.error('Failed to fetch data');
15.	      });
16.	  }
17.	}
18.	
19.	const fetcher = new DataFetcher('https://api.example.com/data');
20.	fetcher.fetchData();


# Solution for a proper error handling - Page 24
1.	class DataFetcher {
2.	  constructor(url) {
3.	    this.url = url;
4.	  }
5.	
6.	  fetchData() {
7.	    fetch(this.url)
8.	      .then(response => {
9.	        if (!response.ok) {
10.	          throw new Error(`HTTP error! status: ${response.status}`);
11.	        }
12.	        return response.json();
13.	      })
14.	      .then(data => {
15.	        console.log('Data fetched:', data);
16.	      })
17.	      .catch(error => {
18.	        console.error(`Failed to fetch data: ${error.message}`);
19.	        this.handleError(error);
20.	      });
21.	  }
22.	
23.	  handleError(error) {
24.	    // Recovery strategy: Retry or provide fallback
25.	    console.log('Retrying fetch or using fallback data...');
26.	    // Example: retry the fetch request up to 3 times
27.	    let attempts = 3;
28.	    const retryFetch = () => {
29.	      if (attempts > 0) {
30.	        attempts--;
31.	        console.log(`Retrying... attempts left: ${attempts}`);
32.	        this.fetchData(); // Retry the request
33.	      } else {
34.	        console.log('All attempts failed. Using fallback data.');
35.	        // Use fallback data to continue the application flow
36.	      }
37.	    };
38.	
39.	    retryFetch();
40.	  }
41.	}
42.	
43.	const fetcher = new DataFetcher('https://api.example.com/data');
44.	fetcher.fetchData();


# Problematic code showing silent failures in Observer pattern - Page 26
1.	class Subject {
2.	  constructor() {
3.	    this.observers = [];
4.	  }
5.	
6.	  addObserver(observer) {
7.	    this.observers.push(observer);
8.	  }
9.	
10.	  notifyObservers(data) {
11.	    this.observers.forEach(observer => {
12.	      observer.update(data);
13.	    });
14.	  }
15.	
16.	  fetchData() {
17.	    try {
18.	      throw new Error('Data fetch failed');
19.	    } catch (error) {
20.	      console.error('Error:', error.message); // Error logged, but observers aren't notified
21.	    }
22.	  }
23.	}
24.	
25.	class Observer {
26.	  update(data) {
27.	    console.log('Received data:', data);
28.	  }
29.	}
30.	
31.	const subject = new Subject();
32.	const observer = new Observer();
33.	
34.	subject.addObserver(observer);
35.	subject.fetchData();


# Solution for silent failures in Observer pattern - Page 27
1.	class Subject {
2.	  constructor() {
3.	    this.observers = [];
4.	  }
5.	
6.	  addObserver(observer) {
7.	    this.observers.push(observer);
8.	  }
9.	
10.	  notifyObservers(data) {
11.	    this.observers.forEach(observer => {
12.	      observer.update(data);
13.	    });
14.	  }
15.	
16.	  notifyError(error) {
17.	    this.observers.forEach(observer => {
18.	      observer.handleError(error);
19.	    });
20.	  }
21.	
22.	  fetchData() {
23.	    try {
24.	      throw new Error('Data fetch failed');
25.	    } catch (error) {
26.	      console.error('Error:', error.message);
27.	      this.notifyError(error); // Notify observers of the error
28.	    }
29.	  }
30.	}
31.	
32.	class Observer {
33.	  update(data) {
34.	    console.log('Received data:', data);
35.	  }
36.	
37.	  handleError(error) {
38.	    console.log('Handling error:', error.message);
39.	  }
40.	}
41.	
42.	const subject = new Subject();
43.	const observer = new Observer();
44.	
45.	subject.addObserver(observer);
46.	subject.fetchData();


# Problematic code showing Singleton pattern with exposed data - Page 29
1.	class Config {
2.	  constructor() {
3.	    if (!Config.instance) {
4.	      this.apiKey = '12345-abcde'; // Sensitive data
5.	      Config.instance = this;
6.	    }
7.	    return Config.instance;
8.	  }
9.	
10.	  getApiKey() {
11.	    return this.apiKey;
12.	  }
13.	}
14.	
15.	const config = new Config();
16.	console.log(config.getApiKey()); // Exposed API Key


# Solution for exposed data in Singleton pattern - Page 29
1.	class Config {
2.	  constructor() {
3.	    if (!Config.instance) {
4.	      this.apiKey = process.env.API_KEY; // Use environment variables
5.	      Config.instance = this;
6.	    }
7.	    return Config.instance;
8.	  }
9.	
10.	  // Do not expose the API key directly
11.	}
12.	
13.	const config = new Config();
14.	console.log('Config created, API key securely stored');


# Problematic code showing XSS issues in Observer pattern - Page 30
1.	class Subject {
2.	  constructor() {
3.	    this.observers = [];
4.	  }
5.	
6.	  addObserver(observer) {
7.	    this.observers.push(observer);
8.	  }
9.	
10.	  notifyObservers(data) {
11.	    this.observers.forEach(observer => observer.update(data));
12.	  }
13.	}
14.	
15.	class CommentObserver {
16.	  update(comment) {
17.	    // Unsanitized input directly inserted into the DOM
18.	    document.body.innerHTML += `<p>${comment}</p>`;
19.	  }
20.	}
21.	
22.	const subject = new Subject();
23.	const observer = new CommentObserver();
24.	
25.	subject.addObserver(observer);
26.	
27.	// Example of XSS attack: malicious comment with a script tag
28.	subject.notifyObservers('<script>alert("XSS Attack!");</script>');


# Solution for XSS issues in Observer pattern - Page 31
1.	class CommentObserver {
2.	  update(comment) {
3.	    // Sanitize input before inserting into the DOM
4.	    const sanitizedComment = DOMPurify.sanitize(comment);
5.	    document.body.innerHTML += `<p>${sanitizedComment}</p>`;
6.	  }
7.	}
8.	
9.	const subject = new Subject();
10.	const observer = new CommentObserver();
11.	
12.	subject.addObserver(observer);
13.	
14.	// Now, the malicious script is neutralized
15.	subject.notifyObservers('<script>alert("XSS Attack!");</script>');


# Problematic code for CSRF issues in Factory pattern - Page 31
1.	class RequestFactory {
2.	  createRequest(type) {
3.	    switch (type) {
4.	      case 'delete':
5.	        return new DeleteRequest();
6.	      default:
7.	        return new GetRequest();
8.	    }
9.	  }
10.	}
11.	
12.	class DeleteRequest {
13.	  send(url) {
14.	    // Sending delete request without CSRF protection
15.	    fetch(url, { method: 'DELETE' })
16.	      .then(response => response.json())
17.	      .then(data => console.log(data))
18.	      .catch(error => console.error('Error:', error));
19.	  }
20.	}
21.	
22.	class GetRequest {
23.	  send(url) {
24.	    fetch(url)
25.	      .then(response => response.json())
26.	      .then(data => console.log(data))
27.	      .catch(error => console.error('Error:', error));
28.	  }
29.	}
30.	
31.	// CSRF attack: unauthorized deletion request
32.	const factory = new RequestFactory();
33.	const deleteRequest = factory.createRequest('delete');
34.	deleteRequest.send('https://api.example.com/user/123'); // Dangerous without CSRF token


# Solution for CSRF issues in Factory pattern - Page 32
1.	class DeleteRequest {
2.	  send(url) {
3.	    // Include CSRF token in sensitive requests
4.	    const csrfToken = document.querySelector('meta[name="csrf-token"]').getAttribute('content');
5.	    
6.	    fetch(url, {
7.	      method: 'DELETE',
8.	      headers: {
9.	        'X-CSRF-Token': csrfToken,
10.	      },
11.	    })
12.	      .then(response => response.json())
13.	      .then(data => console.log(data))
14.	      .catch(error => console.error('Error:', error));
15.	  }
16.	}


# Problematic code showing God Object anti-pattern - Page 33
1.	class GodObject {
2.	  constructor() {
3.	    this.userService = new UserService();
4.	    this.paymentService = new PaymentService();
5.	    this.loggerService = new LoggerService();
6.	    // Many more responsibilities...
7.	  }
8.	
9.	  processOrder(order) {
10.	    // Handles all order processing logic
11.	    this.userService.updateUser(order.user);
12.	    this.paymentService.processPayment(order.payment);
13.	    this.loggerService.logOrder(order);
14.	    // More complex operations...
15.	  }
16.	}


# Solution for God Object anti-pattern - Page 34
1.	class OrderProcessor {
2.	  constructor(userService, paymentService, loggerService) {
3.	    this.userService = userService;
4.	    this.paymentService = paymentService;
5.	    this.loggerService = loggerService;
6.	  }
7.	
8.	  processOrder(order) {
9.	    this.userService.updateUser(order.user);
10.	    this.paymentService.processPayment(order.payment);
11.	    this.loggerService.logOrder(order);
12.	  }
13.	}
14.	
15.	const userService = new UserService();
16.	const paymentService = new PaymentService();
17.	const loggerService = new LoggerService();
18.	
19.	const orderProcessor = new OrderProcessor(userService, paymentService, loggerService);
20.	orderProcessor.processOrder(order);


# Problematic code showing Spaghetti code - Page 35
1.	function placeOrder(order) {
2.	  updateUser(order.user);
3.	  if (order.paymentType === 'card') {
4.	    processCardPayment(order);
5.	  } else {
6.	    processPaypalPayment(order);
7.	  }
8.	
9.	  if (order.items.length > 0) {
10.	    for (let i = 0; i < order.items.length; i++) {
11.	      updateInventory(order.items[i]);
12.	    }
13.	  }
14.	
15.	  logOrder(order);
16.	}


# Solution for Spaghetti code using design patterns - Page 35
1.	class PaymentStrategy {
2.	  processPayment(order) {
3.	    throw new Error("Method not implemented");
4.	  }
5.	}
6.	
7.	class CardPaymentStrategy extends PaymentStrategy {
8.	  processPayment(order) {
9.	    // Card payment logic
10.	  }
11.	}
12.	
13.	class PaypalPaymentStrategy extends PaymentStrategy {
14.	  processPayment(order) {
15.	    // PayPal payment logic
16.	  }
17.	}
18.	
19.	class OrderProcessor {
20.	  constructor(paymentStrategy) {
21.	    this.paymentStrategy = paymentStrategy;
22.	  }
23.	
24.	  processOrder(order) {
25.	    this.paymentStrategy.processPayment(order);
26.	    order.items.forEach(item => updateInventory(item));
27.	    logOrder(order);
28.	  }
29.	}
30.	
31.	const paymentStrategy = new CardPaymentStrategy(); // or new PaypalPaymentStrategy()
32.	const orderProcessor = new OrderProcessor(paymentStrategy);
33.	orderProcessor.processOrder(order);


# Problematic code showing premature optimization
1.	function calculateTotal(items) {
2.	  let total = 0;
3.	  // Optimized prematurely for large arrays
4.	  for (let i = 0, len = items.length; i < len; i++) {
5.	    total += items[i].price;
6.	  }
7.	  return total;
8.	}


# Solution for premature optimization
1.	function calculateTotal(items) {
2.	  return items.reduce((total, item) => total + item.price, 0);
3.	}