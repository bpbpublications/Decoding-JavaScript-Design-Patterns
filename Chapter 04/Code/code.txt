# Problem Statement 1 without Strategy Pattern - Page 2
1.	// Problematic Code without Strategy Pattern
2.	
3.	// Shopping Cart class
4.	class ShoppingCart {
5.	  constructor() {
6.	    this.items = [];
7.	  }
8.	
9.	  // Method to calculate total price
10.	  calculateTotalPrice() {
11.	    let totalPrice = 0;
12.	
13.	    this.items.forEach(item => {
14.	     // Problem: Switch statement for different pricing strategies
15.	      switch (item.type) {
16.	        case 'book':
17.	          totalPrice += item.price * 0.9; // 10% discount on books
18.	          break;
19.	        case 'mobile':
20.	          totalPrice += item.price * 1.2; //20% markup on mobiles
21.	          break;
22.	        // More cases for other types...
23.	      }
24.	    });
25.	
26.	    return totalPrice;
27.	  }
28.	
29.	  // Method to add items to the cart
30.	  addItem(item) {
31.	    this.items.push(item);
32.	  }
33.	}
34.	
35.	// Example usage
36.	const cart = new ShoppingCart();
37.	cart.addItem({ type: 'book', price: 30 });
38.	cart.addItem({ type: 'mobile', price: 50 });
39.	
40.	console.log(`Total Price: $${cart.calculateTotalPrice()}`);


# Problem Statement 2 without Chain of Responsibility Pattern - Page 3
1.	// Problematic Code without Chain of Responsibility Pattern
2.	
3.	// Logger class
4.	class Logger {
5.	  log(message, level) {
6.	    // Problem: Conditional statements for different log levels
7.	    if (level === 'info') {
8.	      console.log(`[INFO] ${message}`);
9.	    } else if (level === 'warning') {
10.	      console.warn(`[WARNING] ${message}`);
11.	    } else if (level === 'error') {
12.	      console.error(`[ERROR] ${message}`);
13.	    }
14.	    // More conditions for other log levels...
15.	  }
16.	}
17.	
18.	// Example usage
19.	const logger = new Logger();
20.	logger.log('System is running smoothly.', 'info');
21.	logger.log('Warning: Low disk space.', 'warning');


# Step 1 of Chain of Responsibility pattern - Page 7
1.	// Handler Interface
2.	class Handler {
3.	  constructor() {
4.	    this.nextHandler = null;
5.	  }
6.	
7.	  setNextHandler(handler) {
8.	    this.nextHandler = handler;
9.	  }
10.	
11.	  handleTicket(ticket) {
12.	    // Handle the ticket or pass it to the next handler
13.	    if (this.nextHandler) {
14.	      this.nextHandler.handleTicket(ticket);
15.	    } else {
16.	      console.log("Ticket not resolved by any handler.");
17.	    }
18.	  }
19.	}


# Step 2 of Chain of Responsibility pattern - Page 8
1.	// Concrete Handlers
2.	class Level1SupportHandler extends Handler {
3.	  handleTicket(ticket) {
4.	  }
5.	}
6.	
7.	class Level2SupportHandler extends Handler {
8.	  handleTicket(ticket) {
9.	  }
10.	}
11.	
12.	class ManagerHandler extends Handler {
13.	  handleTicket(ticket) {
14.	  }
15.	}


# Step 3 of Chain of Responsibility pattern - Page 8
1.	// Concrete Handlers
2.	class Level1SupportHandler extends Handler {
3.	  handleTicket(ticket) {
4.	    if (ticket.level === 1) {
5.	      console.log("Level 1 Support handled the ticket.");
6.	    } else {
7.	      super.handleTicket(ticket);
8.	    }
9.	  }
10.	}
11.	
12.	class Level2SupportHandler extends Handler {
13.	  handleTicket(ticket) {
14.	    if (ticket.level === 2) {
15.	      console.log("Level 2 Support handled the ticket.");
16.	    } else {
17.	      super.handleTicket(ticket);
18.	    }
19.	  }
20.	}
21.	
22.	class ManagerHandler extends Handler {
23.	  handleTicket(ticket) {
24.	    if (ticket.level > 2) {
25.	      console.log("Manager handled the escalated ticket.");
26.	    } else {
27.	      super.handleTicket(ticket);
28.	    }
29.	  }
30.	}


# Step 4 of Chain of Responsibility pattern - Page 9
1.	// Client Code - Set Up the Chain
2.	const level1Handler = new Level1SupportHandler();
3.	const level2Handler = new Level2SupportHandler();
4.	const managerHandler = new ManagerHandler();
5.	
6.	level1Handler.setNextHandler(level2Handler);
7.	level2Handler.setNextHandler(managerHandler);


# Step 5 of Chain of Responsibility pattern - Page 10
1.	// Client Code - Submit Ticket
2.	const userTicket = { level: 2 };
3.	level1Handler.handleTicket(userTicket);


# Step 1 of Command pattern - Page 13
1.	// Command interface
2.	class Command {
3.	  execute() {}
4.	}


# Step 2 of Command pattern - Page 14
1.	// ConcreteCommand for Lights
2.	class LightsOnCommand extends Command {
3.	  constructor(light) {
4.	    super();
5.	    this.light = light;
6.	  }
7.	
8.	  execute() {
9.	    this.light.turnOn();
10.	  }
11.	}
12.	
13.	// ConcreteCommand for Thermostat
14.	class SetTemperatureCommand extends Command {
15.	  constructor(thermostat, temperature) {
16.	    super();
17.	    this.thermostat = thermostat;
18.	    this.temperature = temperature;
19.	  }
20.	
21.	  execute() {
22.	    this.thermostat.setTemperature(this.temperature);
23.	  }
24.	}


# Step 3 of Command pattern - Page 15
1.	// Receiver for Lights
2.	class Light {
3.	  turnOn() {
4.	    console.log("Lights are on.");
5.	  }
6.	}
7.	
8.	// Receiver for Thermostat
9.	class Thermostat {
10.	  setTemperature(temperature) {
11.	    console.log(`Thermostat temperature set to ${temperature} degrees.`);
12.	  }
13.	}


# Step 4 of Command pattern - Page 15
1.	// Invoker
2.	class RemoteControl {
3.	  constructor() {
4.	    this.commands = [];
5.	  }
6.	
7.	  addCommand(command) {
8.	    this.commands.push(command);
9.	  }
10.	
11.	  executeCommands() {
12.	    this.commands.forEach(command => command.execute());
13.	  }
14.	}


# Step 5 of Command pattern - Page 16
1.	// Client code
2.	const livingRoomLights = new Light();
3.	const livingRoomThermostat = new Thermostat();
4.	
5.	const lightsCommand = new LightsOnCommand(livingRoomLights);
6.	const thermostatCommand = new SetTemperatureCommand(livingRoomThermostat, 22);
7.	
8.	const remoteControl = new RemoteControl();
9.	remoteControl.addCommand(lightsCommand);
10.	remoteControl.addCommand(thermostatCommand);
11.	
12.	// Execute the routine
13.	remoteControl.executeCommands();


# Problem Scenario for Iterator pattern - Page 18
1.	const fileSystem = {
2.	  name: 'Root',
3.	  type: 'directory',
4.	  children: [
5.	    {
6.	      name: 'Folder1',
7.	      type: 'directory',
8.	      children: [
9.	        { name: 'File1.txt', type: 'file' },
10.	        { name: 'File2.txt', type: 'file' },
11.	      ],
12.	    },
13.	    {
14.	      name: 'Folder2',
15.	      type: 'directory',
16.	      children: [
17.	        { name: 'File3.txt', type: 'file' },
18.	        { name: 'Folder3', type: 'directory', children: [{ name: 'File4.txt', type: 'file' }] },
19.	      ],
20.	    },
21.	  ],
22.	};
23.	
24.	// Without Iterator
25.	function processFileSystem(node) {
26.	  if (node.type === 'file') {
27.	    console.log(node.name);
28.	    // Perform some operation on the file
29.	  } else if (node.type === 'directory') {
30.	    for (const child of node.children) {
31.	      processFileSystem(child);
32.	    }
33.	  }
34.	}
35.	
36.	processFileSystem(fileSystem);


# Step 1 of Iterator pattern - Page 20
1.	class ShoppingListTemplate {
2.	  nextItem() {}
3.	  hasMoreItems() {}
4.	}


# Step 2 of Iterator pattern - Page 20
1.	class GroceryListIterator extends ShoppingListTemplate {
2.	  constructor(items) {
3.	    super();
4.	    this.items = items;
5.	    this.currentIndex = 0;
6.	  }
7.	
8.	  nextItem() {
9.	    if (this.hasMoreItems()) {
10.	      return this.items[this.currentIndex++];
11.	    }
12.	    return null;
13.	  }
14.	
15.	  hasMoreItems() {
16.	    return this.currentIndex < this.items.length;
17.	  }
18.	}


# Step 3 of Iterator pattern - Page 21
1.	class ShoppingCart {
2.	  constructor(lists) {
3.	    this.lists = lists;
4.	  }
5.	
6.	  getListIterator(listIndex) {
7.	    const list = this.lists[listIndex];
8.	    return new GroceryListIterator(list);
9.	  }
10.	}


# Step 4 of Iterator pattern - Page 22
1.	const shoppingCart = new ShoppingCart([
2.	  ['Apples', 'Bananas', 'Oranges'],
3.	  ['Milk', 'Eggs', 'Bread', 'Butter'],
4.	]);
5.	
6.	const listIterator = shoppingCart.getListIterator(1); // Accessing the second list
7.	
8.	while (listIterator.hasMoreItems()) {
9.	  const item = listIterator.nextItem();
10.	  console.log(item);
11.	  // Purchase or perform some action on the item
12.	}


# Step 1 of Mediator pattern - Page 26
1.	class ATCTower {
2.	  constructor() {
3.	    this.airplanes = [];
4.	  }
5.	
6.	  addAirplane(airplane) {
7.	    this.airplanes.push(airplane);
8.	  }
9.	
10.	  sendMessage(message, sender) {
11.	    this.airplanes.forEach(airplane => {
12.	      if (airplane !== sender) {
13.	        airplane.receiveMessage(message);
14.	      }
15.	    });
16.	  }
17.	}


# Step 2 of Mediator pattern - Page 26
1.	class Airplane {
2.	  constructor(id, atcTower) {
3.	    this.id = id;
4.	    this.atcTower = atcTower;
5.	  }
6.	
7.	  sendMessage(message) {
8.	    this.atcTower.sendMessage(message, this);
9.	  }
10.	
11.	  receiveMessage(message) {
12.	    console.log(`Airplane ${this.id} received: ${message}`);
13.	  }
14.	}


# Step 3 of Mediator pattern - Page 27
1.	// Create an instance of the ATC tower
2.	const atcTower = new ATCTower();
3.	
4.	// Create airplane instances
5.	const airplane1 = new Airplane('ABC123', atcTower);
6.	const airplane2 = new Airplane('XYZ789', atcTower);
7.	const airplane3 = new Airplane('DEF456', atcTower);
8.	
9.	// Register airplanes with the ATC tower
10.	atcTower.addAirplane(airplane1);
11.	atcTower.addAirplane(airplane2);
12.	atcTower.addAirplane(airplane3);


# Step 4 of Mediator pattern - Page 28
1.	airplane1.sendMessage('Requesting permission to land.');
2.	airplane2.sendMessage('Roger, cleared for landing.');
3.	airplane3.sendMessage('Reporting turbulence ahead, advise caution.');


# Step 1 of Memento pattern - Page 31
1.	class Playlist {
2.	  constructor(tracks) {
3.	    this.tracks = tracks;
4.	  }
5.	
6.	  setTracks(tracks) {
7.	    this.tracks = tracks;
8.	  }
9.	
10.	  saveToMemento() {
11.	    return new PlaylistMemento(this.tracks);
12.	  }
13.	
14.	  restoreFromMemento(memento) {
15.	    this.tracks = memento.getTracks();
16.	  }
17.	}


# Step 2 of Memento pattern - Page 32
1.	class PlaylistMemento {
2.	  constructor(tracks) {
3.	    this.tracks = tracks;
4.	  }
5.	
6.	  getTracks() {
7.	    return this.tracks;
8.	  }
9.	}


# Step 3 of Memento pattern - Page 32
1.	class PlaylistCaretaker {
2.	  constructor() {
3.	    this.mementos = [];
4.	  }
5.	
6.	  addMemento(memento) {
7.	    this.mementos.push(memento);
8.	  }
9.	
10.	  getMemento(index) {
11.	    return this.mementos[index];
12.	  }
13.	}


# Step 4 of Memento pattern - Page 33
1.	// Example usage
2.	const playlist = new Playlist(["Song 1", "Song 2", "Song 3"]);
3.	const playlistCaretaker = new PlaylistCaretaker();
4.	
5.	playlistCaretaker.addMemento(playlist.saveToMemento()); // Save initial playlist
6.	
7.	playlist.setTracks(["Song 4", "Song 5"]); // Modify playlist
8.	
9.	playlistCaretaker.addMemento(playlist.saveToMemento()); // Save modified playlist
10.	
11.	playlist.restoreFromMemento(playlistCaretaker.getMemento(0)); // Restore initial playlist
12.	
13.	console.log(playlist.tracks); // Output: ["Song 1", "Song 2", "Song 3"]


# Step 1 of Observer pattern - Page 37
1.	class User {
2.	  constructor(username) {
3.	    this.username = username;
4.	    this.followers = [];
5.	  }
6.	
7.	  follow(follower) {
8.	  }
9.	
10.	  unfollow(follower) {
11.	  }
12.	
13.	  postUpdate(update) {
14.	  }
15.	
16.	  notifyFollowers(update) {
17.	  }
18.	}


# Step 2 of Observer pattern - Page 38
1.	class Follower {
2.	  constructor(name) {
3.	    this.name = name;
4.	  }
5.	
6.	  receiveUpdate(username, update) {
7.	    console.log(`${this.name} received update from ${username}: ${update}`);
8.	  }
9.	}


# Step 3 of Observer pattern - Page 38
1.	class User {
2.	  constructor(username) {
3.	    this.username = username;
4.	    this.followers = [];
5.	  }
6.	
7.	  follow(follower) {
8.	    this.followers.push(follower);
9.	  }
10.	
11.	  unfollow(follower) {
12.	    this.followers = this.followers.filter(f => f !== follower);
13.	  }
14.	
15.	  postUpdate(update) {
16.	  }
17.	
18.	  notifyFollowers(update) {
19.	  }
20.	}


# Step 4 of Observer pattern - Page 39
1.	class User {
2.	  constructor(username) {
3.	    this.username = username;
4.	    this.followers = [];
5.	  }
6.	
7.	  follow(follower) {
8.	    this.followers.push(follower);
9.	  }
10.	
11.	  unfollow(follower) {
12.	    this.followers = this.followers.filter(f => f !== follower);
13.	  }
14.	
15.	  postUpdate(update) {
16.	    console.log(`${this.username} posted update: ${update}`);
17.	    this.notifyFollowers(update);
18.	  }
19.	
20.	  notifyFollowers(update) {
21.	    this.followers.forEach(follower => follower.receiveUpdate(this.username, update));
22.	  }
23.	}


# Step 5 of Observer pattern - Page 40
1.	// Create users
2.	const user1 = new User("Alice");
3.	const user2 = new User("Bob");
4.	
5.	// Create followers
6.	const follower1 = new Follower("Charlie");
7.	const follower2 = new Follower("Diana");
8.	
9.	// Subscribe followers
10.	user1.follow(follower1);
11.	user1.follow(follower2);
12.	user2.follow(follower2);
13.	
14.	// Trigger updates
15.	user1.postUpdate("Hello, world!");
16.	user2.postUpdate("Goodbye, world!");


# Step 1 of Strategy pattern - Page 43
1.	// Strategy 1: Regular shipping strategy
2.	const regularShippingStrategy = {
3.	  calculateCost: (weight, distance) => {
4.	    return weight * 0.5 + distance * 0.1;
5.	  }
6.	};
7.	
8.	// Strategy 2: Express shipping strategy
9.	const expressShippingStrategy = {
10.	  calculateCost: (weight, distance) => {
11.	    return weight * 0.8 + distance * 0.2;
12.	  }
13.	};
14.	
15.	// Strategy 3: Free shipping strategy
16.	const freeShippingStrategy = {
17.	  calculateCost: (weight, distance) => {
18.	    return 0; // Free shipping
19.	  }
20.	};


# Step 2 of Strategy pattern - Page 43
1.	class ShippingCalculator {
2.	  constructor(strategy) {
3.	    this.strategy = strategy;
4.	  }
5.	
6.	  setStrategy(strategy) {
7.	    this.strategy = strategy;
8.	  }
9.	
10.	  calculateShippingCost(weight, distance) {
11.	    return this.strategy.calculateCost(weight, distance);
12.	  }
13.	}


# Step 3 of Strategy pattern - Page 44
1.	// Create a shipping calculator instance with regular shipping strategy
2.	const calculator = new ShippingCalculator(regularShippingStrategy);
3.	
4.	// Calculate shipping cost for a package
5.	const weight = 10; // in kg
6.	const distance = 500; // in km
7.	
8.	const regularShippingCost = calculator.calculateShippingCost(weight, distance);
9.	console.log("Regular shipping cost:", regularShippingCost);
10.	
11.	// Change the strategy to express shipping
12.	calculator.setStrategy(expressShippingStrategy);
13.	
14.	const expressShippingCost = calculator.calculateShippingCost(weight, distance);
15.	console.log("Express shipping cost:", expressShippingCost);
16.	
17.	// Change the strategy to free shipping
18.	calculator.setStrategy(freeShippingStrategy);
19.	
20.	const freeShippingCost = calculator.calculateShippingCost(weight, distance);
21.	console.log("Free shipping cost:", freeShippingCost);
