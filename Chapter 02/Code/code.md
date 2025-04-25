# Object prototype example - Page 5
1.	const person = {
2.	    name: "Rushabh",
3.	    greet() {
4.	        console.log(`Hi, my name is ${this.name}!`);
5.	    }
6.	}
7.	
8.	person.greet();
9.	console.log(person);


# Example of a constructor - Page 7
1.	class Person {
2.	    constructor(name) {
3.	          this.name = name;
4.	    }
5.	
6.	    greet() {
7.	        console.log(`Hi, my name is ${this.name}!`);
8.	    }
9.	}
10.	
11.	const teacher = new Person("Rushabh");
12.	
13.	teacher.greet();


# Constructor in a derived class - Page 8
1.	class Person {
2.	    constructor(name) {
3.	        this.name = name;
4.	    }
5.	    
6.	    introduce() {
7.	        return (`Name of person: ${this.name}`);
8.	    }
9.	}
10.	class Student extends Person {
11.	    constructor(name, id) {
12.	        super(name);
13.	        this.id = id;
14.	    }
15.	    
16.	    introduce() {
17.	        return (`${super.introduce()}, Student ID: ${this.id}`);
18.	    }
19.	}
20.	let student1 = new Student('Mukul', 22);
21.	console.log(student1.introduce());


# Example of inheritance in JavaScript - Page 9
1.	class Person {
2.	    constructor(name) {
3.	        this.name = name;
4.	    }
5.	    
6.	    introduce() {
7.	        return (`Name of person: ${this.name}`);
8.	    }
9.	}
10.	class Student extends Person {
11.	    constructor(name, id) {
12.	        super(name);
13.	        this.id = id;
14.	    }
15.	    
16.	    introduce() {
17.	        return (`${super.introduce()}, Student ID: ${this.id}`);
18.	    }
19.	}
20.	let student1 = new Student('Mukul', 22);
21.	console.log(student1);



# Polymorphism in JavaScript - Page 10
1.	class Person {
2.	  constructor(name) {
3.	    this.name = name;
4.	  }
5.	
6.	  introduce() {
7.	    return `Hello, my name is ${this.name}.`;
8.	  }
9.	}
10.	
11.	class Student extends Person {
12.	  constructor(name, age) {
13.	    super(name);
14.	    this.age = age;
15.	  }
16.	
17.	  introduce() {
18.	    return `Hi, I'm ${this.name}, and I'm ${this.age} years old.`;
19.	  }
20.	}
21.	
22.	// Example usage:
23.	const student = new Student("Mukul", 22);
24.	const person = new Person("Raj");
25.	const introducePeople = [student, person];
26.	
27.	introducePeople.forEach(person => console.log(person.introduce()));


# Example of a memory leak due to incorrect object creation - Page 14
1.	function EventListener() {
2.	    this.eventHandlers = [];
3.	
4.	    this.addEventListener = function (handler) {
5.	        this.eventHandlers.push(handler);
6.	    };
7.	
8.	    this.triggerEvent = function () {
9.	        this.eventHandlers.forEach(handler => handler());
10.	    };
11.	}
12.	
13.	// Usage with memory leak
14.	const listener = new EventListener();
15.	listener.addEventListener(() => console.log('Event triggered'));


# Example of a circular dependency - Page 14
1.	// Module A
2.	const A = require('./B');
3.	
4.	function ModuleA() {
5.	    this.name = 'Module A';
6.	    this.bInstance = new A.ModuleB(); // Circular dependency
7.	}
8.	
9.	// Module B
10.	const B = require('./A');
11.	
12.	function ModuleB() {
13.	    this.name = 'Module B';
14.	    this.aInstance = new B.ModuleA(); // Circular dependency
15.	}
16.	
17.	// Usage
18.	const instanceA = new A.ModuleA(); // Results in a circular dependency error


# Example of an uninitialised variable - Page 15
1.	
function UserProfile(name, age) {
2.	    this.name = name;
3.	    this.age = age;
4.	}
5.	
6.	// Usage with uninitialized variable
7.	const user = new UserProfile(); 


# Code example of Singleton design pattern - Page 18
1.	class SingletonUser {
2.	    static #instance; // Creating a private static variable
3.	    
4.	    constructor(name) {
5.	        if(!SingletonUser.#instance) { //Ensuring that not more than one instance can be created
6.	            SingletonUser.#instance = this;
7.	            this.name = name;
8.	        }
9.	        return SingletonUser.#instance;
10.	    }
11.	
12.	    static getInstance() { //Method to get instance of the class
13.	        if(!SingletonUser.#instance) {
14.	            SingletonUser.#instance = this;
15.	        }
16.	        return SingletonUser.#instance;
17.	    }
18.	}
19.	
20.	let user1 = new SingletonUser('Rushabh');
21.	let user2 = new SingletonUser('Unnati');
22.	console.log("user1:  ", user1);
23.	console.log("user2:  ", user2);
24.	console.log("user1 === user2:   ", user1 === user2);
25.	
26.	let user3 = SingletonUser.getInstance();
27.	let user4 = SingletonUser.getInstance();
28.	console.log("user3:   ", user3);
29.	console.log("user4:   ", user4);
30.	console.log("user3 === user4:   ", user3 === user4);


# Step 1 of Factory Method pattern - Page 22
1.	class Vehicle {
2.	  constructor(type, model) {
3.	    this.type = type;
4.	    this.model = model;
5.	  }
6.	
7.	  display() {
8.	    console.log(`${this.type}: ${this.model}`);
9.	  }
10.	}


# Step 2 of Factory Method pattern - Page 23
1.	class Car extends Vehicle {
2.	    constructor(model) {
3.	        super('Car', model);
4.	    }
5.	}
6.	 
7.	class Bicycle extends Vehicle {
8.	    constructor(model) {
9.	        super('Bicycle', model);
10.	    }
11.	}


# Step 3 of Factory Method pattern - Page 24
1.	class VehicleFactory {
2.	  createVehicle() {
3.	    throw new Error('createVehicle method must be overridden');
4.	  }
5.	
6.	  assemble() {
7.	    const vehicle = this.createVehicle();
8.	    console.log(`Assembling ${vehicle.type}`);
9.	    return vehicle;
10.	  }
11.	}


# Step 4 of Factory Method pattern - Page 25
1.	class CarFactory extends VehicleFactory {
2.	  createVehicle() {
3.	    return new Car('Sedan');
4.	  }
5.	}
6.	
7.	class BicycleFactory extends VehicleFactory {
8.	  createVehicle() {
9.	    return new Bicycle('Mountain Bike');
10.	  }
11.	}


# Step 5 of Factory Method pattern - Page 25
1.	const carFactory = new CarFactory();
2.	const bicycleFactory = new BicycleFactory();
3.	
4.	const car = carFactory.assemble();
5.	const bicycle = bicycleFactory.assemble();
6.	
7.	car.display();       // Output: Car: Sedan
8.	bicycle.display();   


# Step 1 of Abstract Factory pattern - Page 30
1.	// Abstract UI Component A: Button
2.	class Button {
3.	  render() {
4.	    throw new Error('Method render must be implemented.');
5.	  }
6.	}
7.	
8.	// Abstract UI Component B: Checkbox
9.	class Checkbox {
10.	  render() {
11.	    throw new Error('Method render must be implemented.');
12.	  }
13.	}


# Step 2 of Abstract Factory Pattern - Page 30
1.	// Abstract UI Component Factory
2.	class UIComponentFactory {
3.	  createButton() {
4.	    throw new Error('Method createButton must be implemented.');
5.	  }
6.	
7.	  createCheckbox() {
8.	    throw new Error('Method createCheckbox must be implemented.');
9.	  }
10.	}


# Step 3 of Abstract Factory Pattern - Page 31 
1.	// Concrete UI Component A1: RoundedButton
2.	class RoundedButton extends Button {
3.	  render() {
4.	    console.log('Rendering a rounded button.');
5.	  }
6.	}
7.	
8.	// Concrete UI Component B1: FancyCheckbox
9.	class FancyCheckbox extends Checkbox {
10.	  render() {
11.	    console.log('Rendering a fancy checkbox.');
12.	  }
13.	}
14.	
15.	// Concrete UI Component A2: FlatButton
16.	class FlatButton extends Button {
17.	  render() {
18.	    console.log('Rendering a flat button.');
19.	  }
20.	}
21.	
22.	// Concrete UI Component B2: BasicCheckbox
23.	class BasicCheckbox extends Checkbox {
24.	  render() {
25.	    console.log('Rendering a basic checkbox.');
26.	  }
27.	}



# Step 4 of Abstract Factory Pattern - Page 32
1.	// Concrete UI Component Factory 1: ModernUIFactory
2.	class ModernUIFactory extends UIComponentFactory {
3.	  createButton() {
4.	    return new RoundedButton();
5.	  }
6.	
7.	  createCheckbox() {
8.	    return new FancyCheckbox();
9.	  }
10.	}
11.	
12.	// Concrete UI Component Factory 2: ClassicUIFactory
13.	class ClassicUIFactory extends UIComponentFactory {
14.	  createButton() {
15.	    return new FlatButton();
16.	  }
17.	
18.	  createCheckbox() {
19.	    return new BasicCheckbox();
20.	  }
21.	}


# Step 5 of Abstract Factory pattern - Page 33
1.	function renderUI(factory) {
2.	  const button = factory.createButton();
3.	  const checkbox = factory.createCheckbox();
4.	
5.	  button.render();
6.	  checkbox.render();
7.	}
8.	
9.	// Usage
10.	const modernUIFactory = new ModernUIFactory();
11.	renderUI(modernUIFactory);
12.	
13.	const classicUIFactory = new ClassicUIFactory();
14.	renderUI(classicUIFactory);


# Step 1 of Builder design pattern - Page 39
1.	// Product: Computer
2.	class Computer {
3.	  constructor() {
4.	    this.cpu = null;
5.	    this.gpu = null;
6.	    this.memory = null;
7.	    this.storage = null;
8.	  }
9.	
10.	  show() {
11.	    console.log(`Computer Specs: CPU - ${this.cpu}, GPU - ${this.gpu}, Memory - ${this.memory}, Storage - ${this.storage}`);
12.	  }
13.	}

# Step 2 of Builder design pattern - Page 40
1.	// Builder Interface: ComputerBuilder
2.	class ComputerBuilder {
3.	  constructor() {
4.	    this.computer = new Computer();
5.	  }
6.	
7.	  buildCPU() {}
8.	  buildGPU() {}
9.	  buildMemory() {}
10.	  buildStorage() {}
11.	  getResult () {
12.	    return this.computer;
13.	  }
14.	}


# Step 3 of Builder design pattern - Page 40
1.	// ConcreteBuilder: GamingComputerBuilder
2.	class GamingComputerBuilder extends ComputerBuilder {
3.	  buildCPU() {
4.	    this.computer.cpu = "Intel Core i9";
5.	  }
6.	
7.	  buildGPU() {
8.	    this.computer.gpu = "NVIDIA GeForce RTX 3080";
9.	  }
10.	
11.	  buildMemory() {
12.	    this.computer.memory = "32GB DDR4 RAM";
13.	  }
14.	
15.	  buildStorage() {
16.	    this.computer.storage = "1TB SSD";
17.	  }
18.	}


# Step 4 of Builder design pattern - Page 41
1.	// Director: ComputerAssemblyDirector
2.	class ComputerAssemblyDirector {
3.	  constructor(builder) {
4.	    this.builder = builder;
5.	  }
6.	
7.	  assembleComputer() {
8.	    this.builder.buildCPU();
9.	    this.builder.buildGPU();
10.	    this.builder.buildMemory();
11.	    this.builder.buildStorage();
12.	  }
13.	}


# Step 5 of Builder design pattern - Page 42
1.	const gamingComputerBuilder = new GamingComputerBuilder();
2.	const assemblyDirector = new ComputerAssemblyDirector(gamingComputerBuilder);
3.	
4.	assemblyDirector.assembleComputer();
5.	const gamingComputer = gamingComputerBuilder.getResult();
6.	gamingComputer.show();


# Adding two more builder classes in Builder design pattern - Page 43
1.	// ConcreteBuilder: OfficeComputerBuilder
2.	class OfficeComputerBuilder extends ComputerBuilder {
3.	  buildCPU() {
4.	    this.computer.cpu = "Intel Core i5";
5.	  }
6.	
7.	  buildGPU() {
8.	    this.computer.gpu = "Integrated Graphics";
9.	  }
10.	
11.	  buildMemory() {
12.	    this.computer.memory = "16GB DDR4 RAM";
13.	  }
14.	
15.	  buildStorage() {
16.	    this.computer.storage = "500GB HDD";
17.	  }
18.	}
19.	
20.	// ConcreteBuilder: BasicComputerBuilder
21.	class BasicComputerBuilder extends ComputerBuilder {
22.	  buildCPU() {
23.	    this.computer.cpu = "Intel Core i3";
24.	  }
25.	
26.	  // No dedicated GPU for BasicComputer
27.	  // This method is intentionally left empty
28.	  buildGPU() {}
29.	
30.	  buildMemory() {
31.	    this.computer.memory = "8GB DDR4 RAM";
32.	  }
33.	
34.	  buildStorage() {
35.	    this.computer.storage = "256GB SSD";
36.	  }
37.	}
38.	
39.	const officeComputerBuilder = new OfficeComputerBuilder();
40.	const officeDirector = new ComputerAssemblyDirector(officeComputerBuilder);
41.	officeDirector.assembleComputer();
42.	const officeComputer = officeComputerBuilder.getResult();
43.	officeComputer.show();
44.	
45.	const basicComputerBuilder = new BasicComputerBuilder();
46.	const basicComputerDirector = new ComputerAssemblyDirector(basicComputerBuilder);
47.	basicComputerDirector.assembleComputer();
48.	const basicComputer = basicComputerBuilder.getResult();
49.	basicComputer.show();


# Step 1 of Prototype design pattern - Page 47
1.	class Shape {
2.	    constructor(color) {
3.	        this.color = color || "default";
4.	    }
5.	
6.	    draw() {
7.	        console.log(`Drawing a ${this.color} shape.`);
8.	    }
9.	}


# Step 2 of Prototype design pattern - Page 48
1.	const circle = new Shape("red");
2.	const square = new Shape("blue");
3.	const triangle = new Shape("green");


# Step 3 of Prototype design pattern - Page 48
1.	circle.radius = 10;
2.	square.sideLength = 15;
3.	triangle.hasHypotenuse = true;


# Step 4 of Prototype design pattern - Page 48
1.	circle.draw();    // Output: Drawing a red shape.
2.	square.draw();    // Output: Drawing a blue shape.
3.	triangle.draw();  // Output: Drawing a green shape.