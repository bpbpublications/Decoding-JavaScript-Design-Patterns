# Problem Statement 1 - Page 2
1.	const person = {
2.	  name: 'John',
3.	  address: {
4.	    street: '123 Main St',
5.	    city: 'Seattle',
6.	    country: 'USA'
7.	  }
8.	};
9.	
10.	// Accessing the country property
11.	const country = person.address.country;


# Problem Statement 2 - Page 3
1.	class BasicCar {
2.	  constructor() {
3.	    this.start = function() {
4.	      console.log('Basic car started.');
5.	    };
6.	  }
7.	}
8.	
9.	class LuxuryCar {
10.	  constructor() {
11.	    this.start = function() {
12.	      console.log('Luxury car started.');
13.	    };
14.	
15.	    this.performLuxuryAction = function() {
16.	      console.log('Performing luxury action.');
17.	    };
18.	  }
19.	}
20.	
21.	// Usage
22.	const car = new LuxuryCar();
23.	car.start();
24.	car.performLuxuryAction(); // Throws an error if used with BasicCar


# Problem Statement 3 - Page 4
1.	class Animal {
2.	  constructor(name) {
3.	    this.name = name;
4.	  }
5.	
6.	  eat() {
7.	    console.log(`${this.name} is eating.`);
8.	  }
9.	}
10.	
11.	class Bird extends Animal {
12.	  fly() {
13.	    console.log(`${this.name} is flying.`);
14.	  }
15.	}
16.	
17.	// Usage
18.	const sparrow = new Bird('Sparrow');
19.	sparrow.eat();


# Step 1 of Adapter design pattern - Page 8
1.	class LegacyAuthenticator {
2.	  authenticate(username, customToken) {
3.	    // Legacy authentication logic
4.	    return "Legacy Authentication Successful";
5.	  }
6.	}


# Step 2 of Adapter design pattern - Page 9
1.	class ModernAuthenticator {
2.	  login(username, password) {
3.	    // The client expects this method
4.	  }
5.	}


# Step 3 of Adapter design pattern - Page 9
1.	class LegacyToModernAuthAdapter extends ModernAuthenticator {
2.	  constructor(legacyAuthenticator) {
3.	    super();
4.	    this.legacyAuthenticator = legacyAuthenticator;
5.	  }
6.	
7.	  login(username, password) {
8.	    // Implement the login method using the legacy authenticator
9.	    return this.legacyAuthenticator.authenticate(username, password);
10.	  }
11.	}


# Step 4 of Adapter design pattern - Page 10
1.	function handleAuthentication(authenticator) {
2.	  console.log(authenticator.login("user123", "password123"));
3.	}


# Step 5 of Adapter design pattern - Page 11
1.	// Instantiate the existing class (legacy authenticator)
2.	const oldAuthenticator = new LegacyAuthenticator();
3.	
4.	// Create an adapter and pass the existing class instance to it
5.	const adaptedAuthenticator = new LegacyToModernAuthAdapter(oldAuthenticator);
6.	
7.	// Use the client code to handle authentication with the adapted authenticator
8.	handleAuthentication(adaptedAuthenticator);


# Step 1 of Bridge design pattern - Page 15
1.	// Implementor interface
2.	class MessageSender {
3.	  sendMessage(message, to) {
4.	    // Implemented by concrete implementors
5.	  }
6.	}


# Step 2 of Bridge design pattern - Page 15
1.	// Concrete Implementor A
2.	class EmailSender extends MessageSender {
3.	  sendMessage(message, to) {
4.	    console.log(`Sending email: "${message}" to ${to}`);
5.	  }
6.	}
7.	
8.	// Concrete Implementor B
9.	class SMSSender extends MessageSender {
10.	  sendMessage(message, to) {
11.	    console.log(`Sending SMS: "${message}" to ${to}`);
12.	  }
13.	}
14.	
15.	// Concrete Implementor C
16.	class SlackSender extends MessageSender {
17.	  sendMessage(message, to) {
18.	    console.log(`Sending Slack message: "${message}" to ${to}`);
19.	  }
20.	}


# Step 3 of Bridge design pattern - Page 16
1.	// Abstraction
2.	class Message {
3.	  constructor(sender) {
4.	    this.sender = sender;
5.	  }
6.	
7.	  send(message, to) {
8.	    // Delegated to the concrete implementor
9.	    this.sender.sendMessage(message, to);
10.	  }
11.	}


# Step 4 of Bridge design pattern - Page 17
1.	// Refined Abstraction A
2.	class EmailMessage extends Message {
3.	  constructor(sender) {
4.	    super(sender);
5.	  }
6.	}
7.	
8.	// Refined Abstraction B 
9.	class SMSMessage extends Message {
10.	  constructor(sender) {
11.	    super(sender);
12.	  }
13.	}
14.	
15.	// Refined Abstraction C (You can add more refined abstractions as needed)
16.	class SlackMessage extends Message {
17.	  constructor(sender) {
18.	    super(sender);
19.	  }
20.	}


# Step 5 of Bridge design pattern - Page 18
1.	// Example usage
2.	const emailSender = new EmailSender();
3.	const smsSender = new SMSSender();
4.	const slackSender = new SlackSender();
5.	
6.	const emailMessage = new EmailMessage(emailSender);
7.	const smsMessage = new SMSMessage(smsSender);
8.	const slackMessage = new SlackMessage(slackSender);
9.	
10.	emailMessage.send("Hello via email!", "john@example.com");
11.	smsMessage.send("Hello via SMS!", "+123456789");
12.	slackMessage.send("Hello via Slack!", "John Doe");


# Step 1 of Composite design pattern - Page 21
1.	class Component {
2.	  constructor(name) {
3.	    this.name = name;
4.	  }
5.	
6.	  // Declare common operations
7.	  operation() {
8.	    throw new Error("Operation must be implemented by subclasses");
9.	  }
10.	}


# Step 2 of Composite design pattern - Page 22
1.	class File extends Component {
2.	  constructor(name, size) {
3.	    super(name);
4.	    this.size = size;
5.	  }
6.	
7.	  operation() {
8.	    console.log(`File: ${this.name}, Size: ${this.size} KB`);
9.	  }
10.	}


# Step 3 of Composite design pattern - Page 22
1.	class Folder extends Component {
2.	  constructor(name) {
3.	    super(name);
4.	    this.children = [];
5.	  }
6.	
7.	  // Add a child component
8.	  add(child) {
9.	    this.children.push(child);
10.	  }
11.	
12.	  // Remove a child component
13.	  remove(child) {
14.	    const index = this.children.indexOf(child);
15.	    if (index !== -1) {
16.	      this.children.splice(index, 1);
17.	    }
18.	  }
19.	
20.	  // Implement operation to traverse and perform actions on children
21.	  operation() {
22.	    console.log(`Folder: ${this.name}`);
23.	    for (const child of this.children) {
24.	      child.operation();
25.	    }
26.	  }
27.	}


# Step 4 of Composite design pattern - Page 23
1.	const rootDirectory = new Folder("Root");
2.	
3.	const file1 = new File("Document.txt", 200);
4.	const file2 = new File("Image.jpg", 500);
5.	
6.	const subDirectory = new Folder("SubDirectory");
7.	const file3 = new File("Code.js", 300);
8.	
9.	subDirectory.add(file3);
10.	
11.	rootDirectory.add(file1);
12.	rootDirectory.add(file2);
13.	rootDirectory.add(subDirectory);
14.	
15.	// Perform operations on the composite structure
16.	rootDirectory.operation();


# Step 5 of Composite design pattern - Page 24
1.	Directory: Root
2.	File: Document.txt, Size: 200 KB
3.	File: Image.jpg, Size: 500 KB
4.	Directory: SubDirectory
5.	File: Code.js, Size: 300 KB


# Step 1 of Decorator design pattern - Page 27
1.	class Coffee {
2.	  cost() {
3.	    return 5;
4.	  }
5.	}


# Step 2 of Decorator design pattern - Page 27
1.	class SimpleCoffee extends Coffee {
2.	  // You can override the methods if needed
3.	  cost() {
4.	    return super.cost();
5.	  }
6.	}


# Step 3 of Decorator design pattern - Page 28
1.	class CoffeeDecorator extends Coffee {
2.	  constructor(coffee) {
3.	    super();
4.	    this._coffee = coffee;
5.	  }
6.	
7.	  cost() {
8.	    return this._coffee.cost();
9.	  }
10.	}


# Step 4 of Decorator design pattern - Page 28
1.	class MilkDecorator extends CoffeeDecorator {
2.	  cost() {
3.	    return super.cost() + 2;
4.	  }
5.	}
6.	
7.	class SugarDecorator extends CoffeeDecorator {
8.	  cost() {
9.	    return super.cost() + 1;
10.	  }
11.	}


# Step 5 of Decorator design pattern - Page 29
1.	// Usage
2.	const simpleCoffee = new SimpleCoffee();
3.	console.log("Cost of simple coffee:", simpleCoffee.cost());
4.	
5.	const milkCoffee = new MilkDecorator(simpleCoffee);
6.	console.log("Cost of coffee with milk:", milkCoffee.cost());
7.	
8.	const sugarMilkCoffee = new SugarDecorator(milkCoffee);
9.	console.log("Cost of coffee with sugar and milk:", sugarMilkCoffee.cost());


# Step 1 of Façade design pattern - Page 33
1.	class FacebookAPI {
2.	  postToFacebook() {
3.	    // Implementation specific to posting on Facebook
4.	  }
5.	}
6.	
7.	class TwitterAPI {
8.	  tweet() {
9.	    // Implementation specific to tweeting on Twitter
10.	  }
11.	}
12.	
13.	class InstagramAPI {
14.	  shareOnInstagram() {
15.	    // Implementation specific to sharing on Instagram
16.	  }
17.	}


# Step 2 of Façade design pattern - Page 34
1.	class SocialMediaFacade {
2.	  constructor() {
3.	    this.facebookAPI = new FacebookAPI();
4.	    this.twitterAPI = new TwitterAPI();
5.	    this.instagramAPI = new InstagramAPI();
6.	  }
7.	
8.	  // Methods to be exposed to the client will be implemented here
9.	}


# Step 3 of Façade design pattern - Page 34
1.	class SocialMediaFacade {
2.	  constructor() {
3.	    this.facebookAPI = new FacebookAPI();
4.	    this.twitterAPI = new TwitterAPI();
5.	    this.instagramAPI = new InstagramAPI();
6.	  }
7.	
8.	  // Encapsulate the logic for sharing on all platforms
9.	  shareOnAllPlatforms() {
10.	    const facebookResult = this.facebookAPI.postToFacebook();
11.	    const twitterResult = this.twitterAPI.tweet();
12.	    const instagramResult = this.instagramAPI.shareOnInstagram();
13.	
14.	    return { facebookResult, twitterResult, instagramResult };
15.	  }
16.	}


# Step 5 of Façade design pattern - Page 35
1.	const socialMediaFacade = new SocialMediaFacade();
2.	const results = socialMediaFacade.shareOnAllPlatforms();
3.	console.log(results);


# Step 2 of Flyweight design pattern - Page 39
1.	// Flyweight interface
2.	class SongFlyweight {
3.	  constructor() {
4.	    this.intrinsicState = null; // Shared properties
5.	  }
6.	
7.	  operation(extrinsicState) {
8.	    // Shared behaviour using intrinsic and extrinsic states
9.	  }
10.	}


# Step 3 of Flyweight design pattern - Page 39
1.	// Concrete Flyweight
2.	class ConcreteSongFlyweight extends SongFlyweight {
3.	  constructor(title, artist, duration) {
4.	    super();
5.	    this.intrinsicState = { title, artist, duration };
6.	  }
7.	
8.	  operation(extrinsicState) {
9.	    // Implement shared behavior using intrinsic and extrinsic states
10.	  }
11.	}


# Step 4 of Flyweight design pattern - Page 40
1.	class SongFlyweightFactory {
2.	  constructor() {
3.	    this.flyweights = {};
4.	  }
5.	
6.	  getSongFlyweight(title, artist, duration) {
7.	    const key = `${title}-${artist}-${duration}`;
8.	    if (!this.flyweights[key]) {
9.	      this.flyweights[key] = new SongFlyweight(title, artist, duration);
10.	    }
11.	    return this.flyweights[key];
12.	  }
13.	}


# Step 5 of Flyweight design pattern - Page 40
1.	// Client code
2.	const songFlyweightFactory = new SongFlyweightFactory();
3.	
4.	const song1 = songFlyweightFactory.getSongFlyweight('Song A', 'Artist X', '3:30');
5.	const song2 = songFlyweightFactory.getSongFlyweight('Song B', 'Artist Y', '4:15');
6.	
7.	song1.operation({ order: 1, playCount: 10 });
8.	song2.operation({ order: 2, playCount: 5 });


# Step 1 of Proxy design pattern - Page 43
1.	// Real Object (Subject)
2.	class ImageLoader {
3.	  loadImage(url) {
4.	    console.log(`ImageLoader: Loading image from ${url}`);
5.	    // Actual image loading logic
6.	  }
7.	}


# Step 2 of Proxy design pattern - Page 44
1.	// Proxy
2.	class ImageProxy {
3.	  constructor() {
4.	    this.imageLoader = new ImageLoader();
5.	  }
6.	
7.	  loadImage(url) {
8.	    // Proxy logic for lazy-loading
9.	    console.log(`ImageProxy: Request received for image from ${url}`);
10.	    // Additional proxy logic for lazy-loading
11.	    this.imageLoader.loadImage(url);
12.	  }
13.	}


# Step 3 of Proxy design pattern - Page 44
1.	// Interface for both Real Object and Proxy
2.	class ImageInterface {
3.	  loadImage(url) {
4.	    // Common method for image loading
5.	  }
6.	}


# Step 4 of Proxy design pattern - Page 45
1.	// Real Object (Subject)
2.	class ImageLoader extends ImageInterface {
3.	  loadImage(url) {
4.	    console.log(`ImageLoader: Loading image from ${url}`);
5.	    // Actual image loading logic
6.	  }
7.	}


# Step 5 of Proxy design pattern - Page 45
1.	// Proxy
2.	class ImageProxy extends ImageInterface {
3.	  constructor() {
4.	    super();
5.	    this.imageLoader = new ImageLoader();
6.	  }
7.	
8.	  loadImage(url) {
9.	    // Proxy logic for lazy-loading
10.	    console.log(`ImageProxy: Request received for image from ${url}`);
11.	    // Additional proxy logic for lazy-loading
12.	    this.imageLoader.loadImage(url);
13.	  }
14.	}


# Step 6 of Proxy design pattern - Page 46
1.	// Client code
2.	const imageLoader = new ImageProxy();
3.	imageLoader.loadImage("example.jpg");


