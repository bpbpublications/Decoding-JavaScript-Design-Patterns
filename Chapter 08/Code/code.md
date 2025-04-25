# Revealing Module Pattern - Page 3
1.	const AuthModule = (() => {
2.	    // Private variables
3.	    let loggedIn = false;
4.	    let username = '';
5.	
6.	    // Private function
7.	    function login(user) {
8.	        loggedIn = true;
9.	        username = user;
10.	        console.log(`${username} logged in successfully.`);
11.	    }
12.	
13.	    function logout() {
14.	        loggedIn = false;
15.	        username = '';
16.	        console.log('Logged out successfully.');
17.	    }
18.	
19.	    // Public members (revealed)
20.	    return {
21.	        isLoggedIn: () => loggedIn,
22.	        getUsername: () => username,
23.	        login,
24.	        logout
25.	    };
26.	})();
27.	
28.	// Usage
29.	console.log(AuthModule.isLoggedIn()); // Output: false
30.	AuthModule.login('john_doe');
31.	console.log(AuthModule.isLoggedIn()); // Output: true
32.	console.log(AuthModule.getUsername()); // Output: "john_doe"
33.	AuthModule.logout();
34.	console.log(AuthModule.isLoggedIn()); // Output: false


# MVC Pattern - Page 7
Model:
1.	class TodoModel {  
2.	  constructor() {  
3.	    this.todos = [];  
4.	  }  
5.	
6.	  addTodo(todo) {  
7.	    this.todos.push(todo);  
8.	  }  
9.	
10.	  removeTodo(index) {  
11.	    this.todos.splice(index, 1);  
12.	  }  
13.	
14.	  getAllTodos() {  
15.	    return this.todos;  
16.	  }  
17.	}  

View:
1.	class TodoView {  
2.	  constructor() {  
3.	    this.todoList = document.getElementById('todo-list');  
4.	  }  
5.	
6.	  renderTodos(todos) {  
7.	    this.todoList.innerHTML = todos  
8.	      .map((todo, index) => `<li>${todo} <button data-index="${index}">Remove</button></li>`)  
9.	      .join('');  
10.	  }  
11.	}  

Controller:
1.	class TodoController {  
2.	  constructor() {  
3.	    this.model = new TodoModel();  
4.	    this.view = new TodoView();  
5.	    this.bindEvents();  
6.	  }  
7.	
8.	  bindEvents() {  
9.	    const addButton = document.getElementById('add-button');  
10.	    const inputField = document.getElementById('todo-input');  
11.	
12.	    // Add Todo Event  
13.	    addButton.addEventListener('click', () => {  
14.	      const todoText = inputField.value.trim();  
15.	      if (todoText) {  
16.	        this.model.addTodo(todoText);  
17.	        this.view.renderTodos(this.model.getAllTodos());  
18.	        inputField.value = '';  
19.	      }  
20.	    });  
21.	
22.	    // Remove Todo Event (Event Delegation)  
23.	    this.view.todoList.addEventListener('click', (event) => {  
24.	      if (event.target.tagName === 'BUTTON') {  
25.	        const index = parseInt(event.target.getAttribute('data-index'), 10);  
26.	        this.model.removeTodo(index);  
27.	        this.view.renderTodos(this.model.getAllTodos());  
28.	      }  
29.	    });  
30.	  }  
31.	}  
32.	
33.	// Initialize the application  
34.	const controller = new TodoController();  

Sample HTML Structure:
1.	<!DOCTYPE html>  
2.	<html lang="en">  
3.	<head>  
4.	  <meta charset="UTF-8">  
5.	  <meta name="viewport" content="width=device-width, initial-scale=1.0">  
6.	  <title>Todo MVC Example</title>  
7.	</head>  
8.	<body>  
9.	  <div>  
10.	    <input type="text" id="todo-input" placeholder="Enter a new todo" />  
11.	    <button id="add-button">Add</button>  
12.	  </div>  
13.	  <ul id="todo-list"></ul>  
14.	  <script src="app.js"></script>  
15.	</body>  
16.	</html>  


# MVP Pattern - Page 11
1.	// Model
2.	class Model {
3.	  constructor() {
4.	    this.data = [];
5.	  }
6.	
7.	  addData(item) {
8.	    this.data.push(item);
9.	  }
10.	
11.	  getAllData() {
12.	    return this.data;
13.	  }
14.	}
15.	
16.	// View
17.	class View {
18.	  constructor() {
19.	    this.list = document.getElementById('list');
20.	    this.input = document.getElementById('input');
21.	    this.addButton = document.getElementById('addBtn');
22.	  }
23.	
24.	  getInput() {
25.	    return this.input.value;
26.	  }
27.	
28.	  render(data) {
29.	    this.list.innerHTML = '';
30.	    data.forEach(item => {
31.	      const li = document.createElement('li');
32.	      li.textContent = item;
33.	      this.list.appendChild(li);
34.	    });
35.	  }
36.	
37.	  setAddButtonHandler(handler) {
38.	    this.addButton.addEventListener('click', handler);
39.	  }
40.	}
41.	
42.	// Presenter
43.	class Presenter {
44.	  constructor(model, view) {
45.	    this.model = model;
46.	    this.view = view;
47.	
48.	    this.view.setAddButtonHandler(this.handleAddButton.bind(this));
49.	    this.updateView();
50.	  }
51.	
52.	  handleAddButton() {
53.	    const input = this.view.getInput();
54.	    if (input) {
55.	      this.model.addData(input);
56.	      this.updateView();
57.	    }
58.	  }
59.	
60.	  updateView() {
61.	    const data = this.model.getAllData();
62.	    this.view.render(data);
63.	  }
64.	}
65.	
66.	// Initialization
67.	document.addEventListener('DOMContentLoaded', () => {
68.	  const model = new Model();
69.	  const view = new View();
70.	  const presenter = new Presenter(model, view);
71.	});


# MVMM Pattern - Page 15
1.	// Mediated Model
2.	class MediatedUserModel {
3.	  constructor(model) {
4.	    this.model = model;
5.	  }
6.	
7.	  setAge(age) {
8.	    this.model.setAge(age);
9.	  }
10.	}
11.	
12.	// Model
13.	class UserModel {
14.	  constructor(name) {
15.	    this.name = name;
16.	    this.age = 0;
17.	  }
18.	
19.	  setAge(age) {
20.	    this.age = age;
21.	    // Notify the mediator that data has changed
22.	    mediator.onDataChange();
23.	  }
24.	}
25.	
26.	// View
27.	class UserView {
28.	  constructor() {
29.	    this.nameInput = document.getElementById('nameInput');
30.	    this.ageDisplay = document.getElementById('ageDisplay');
31.	    this.submitButton = document.getElementById('submitButton');
32.	  }
33.	
34.	  bindSubmit(handler) {
35.	    this.submitButton.addEventListener('click', () => {
36.	      const name = this.nameInput.value;
37.	      handler(name);
38.	    });
39.	  }
40.	
41.	  displayAge(age) {
42.	    this.ageDisplay.textContent = age;
43.	  }
44.	}
45.	
46.	// Mediator
47.	class UserMediator {
48.	  constructor(model, view) {
49.	    this.model = new MediatedUserModel(model);
50.	    this.view = view;
51.	
52.	    // Bind view events to mediator methods
53.	    this.view.bindSubmit(this.updateName.bind(this));
54.	
55.	    // Initial update of view
56.	    this.updateView();
57.	  }
58.	
59.	  updateName(name) {
60.	    this.model.model.name = name;
61.	    // Update the model with new age, triggering a change
62.	    this.model.setAge(Math.floor(Math.random() * 100));
63.	  }
64.	
65.	  onDataChange() {
66.	    this.updateView();
67.	  }
68.	
69.	  updateView() {
70.	    this.view.displayAge(this.model.model.age);
71.	  }
72.	}
73.	
74.	// Usage
75.	const userModel = new UserModel('John');
76.	const userView = new UserView();
77.	const mediator = new UserMediator(userModel, userView);


# Singleton + Factory Design pattern - Page 19
1.	// Singleton pattern implementation for the factory
2.	class SingletonFactory {
3.	  constructor() {
4.	    if (!SingletonFactory.instance) {
5.	      SingletonFactory.instance = this;
6.	    }
7.	    return SingletonFactory.instance;
8.	  }
9.	
10.	  createProduct(type) {
11.	    let product;
12.	    switch(type) {
13.	      case 'A':
14.	        product = new ProductA();
15.	        break;
16.	      case 'B':
17.	        product = new ProductB();
18.	        break;
19.	      default:
20.	        throw new Error('Unknown product type');
21.	    }
22.	    return product;
23.	  }
24.	}
25.	
26.	const instance = new SingletonFactory();
27.	Object.freeze(instance);
28.	
29.	// Product classes
30.	class ProductA {
31.	  constructor() {
32.	    this.name = 'ProductA';
33.	  }
34.	  display() {
35.	    console.log('This is Product A');
36.	  }
37.	}
38.	
39.	class ProductB {
40.	  constructor() {
41.	    this.name = 'ProductB';
42.	  }
43.	  display() {
44.	    console.log('This is Product B');
45.	  }
46.	}
47.	
48.	// Usage
49.	const factory = new SingletonFactory();
50.	const productA = factory.createProduct('A');
51.	const productB = factory.createProduct('B');
52.	
53.	productA.display(); // Output: This is Product A
54.	productB.display(); // Output: This is Product B



# Observer + Mediator design pattern - Page 21
1.	class Mediator {
2.	  constructor() {
3.	    this.channels = {};
4.	  }
5.	
6.	  subscribe(channel, context, func) {
7.	    if (!this.channels[channel]) {
8.	      this.channels[channel] = [];
9.	    }
10.	    this.channels[channel].push({ context, func });
11.	  }
12.	
13.	  publish(channel, ...args) {
14.	    if (!this.channels[channel]) {
15.	      return false;
16.	    }
17.	    this.channels[channel].forEach(subscriber => {
18.	      subscriber.func.apply(subscriber.context, args);
19.	    });
20.	  }
21.	}
22.	
23.	class Component {
24.	  constructor(name, mediator) {
25.	    this.name = name;
26.	    this.mediator = mediator;
27.	  }
28.	
29.	  notify(event, data) {
30.	    this.mediator.publish(event, data);
31.	  }
32.	
33.	  subscribe(event, func) {
34.	    this.mediator.subscribe(event, this, func);
35.	  }
36.	}
37.	
38.	// Usage
39.	const mediator = new Mediator();
40.	
41.	const componentA = new Component('ComponentA', mediator);
42.	const componentB = new Component('ComponentB', mediator);
43.	
44.	componentA.subscribe('eventX', data => console.log(`${componentA.name} received: ${data}`));
45.	componentB.subscribe('eventX', data => console.log(`${componentB.name} received: ${data}`));
46.	
47.	componentA.notify('eventX', 'Hello from ComponentA');
48.	componentB.notify('eventX', 'Hello from ComponentB');


# Decorator + Strategy pattern - Page 23
1.	// Strategy pattern
2.	class Strategy {
3.	  execute(data) {
4.	    throw new Error('Strategy#execute needs to be overridden');
5.	  }
6.	}
7.	
8.	class ConcreteStrategyA extends Strategy {
9.	  execute(data) {
10.	    console.log('Strategy A executed with data:', data);
11.	  }
12.	}
13.	
14.	class ConcreteStrategyB extends Strategy {
15.	  execute(data) {
16.	    console.log('Strategy B executed with data:', data);
17.	  }
18.	}
19.	
20.	class Context {
21.	  constructor(strategy) {
22.	    this.strategy = strategy;
23.	  }
24.	
25.	  setStrategy(strategy) {
26.	    this.strategy = strategy;
27.	  }
28.	
29.	  executeStrategy(data) {
30.	    this.strategy.execute(data);
31.	  }
32.	}
33.	
34.	// Decorator to add logging
35.	class LoggingDecorator {
36.	  constructor(context) {
37.	    this.context = context;
38.	  }
39.	
40.	  executeStrategy(data) {
41.	    console.log('Logging before execution');
42.	    this.context.executeStrategy(data);
43.	    console.log('Logging after execution');
44.	  }
45.	}
46.	
47.	// Usage
48.	const strategyA = new ConcreteStrategyA();
49.	const context = new Context(strategyA);
50.	
51.	const loggedContext = new LoggingDecorator(context);
52.	loggedContext.executeStrategy('Test data'); // Logs around strategy execution
53.	
54.	context.setStrategy(new ConcreteStrategyB());
55.	loggedContext.executeStrategy('Test data'); // Logs around new strategy execution


