# Sample code example - Page 9
1.	// Subject
2.	class Subject {
3.	  constructor() {
4.	    this.observers = [];
5.	  }
6.	
7.	  // Method to attach observers
8.	  attach(observer) {
9.	    this.observers.push(observer);
10.	  }
11.	
12.	  // Method to detach observers
13.	  detach(observer) {
14.	    this.observers = this.observers.filter(obs => obs !== observer);
15.	  }
16.	
17.	  // Method to notify all observers
18.	  notify() {
19.	    this.observers.forEach(observer => observer.update());
20.	  }
21.	
22.	  // Example state change
23.	  someBusinessLogic() {
24.	    console.log('Subject: Doing something important...');
25.	    this.notify();
26.	  }
27.	}
28.	
29.	// Observer
30.	class Observer {
31.	  constructor(name) {
32.	    this.name = name;
33.	  }
34.	
35.	  // Update method to be called when the subject changes state
36.	  update() {
37.	    console.log(`${this.name} has been notified.`);
38.	  }
39.	}
40.	
41.	// Usage
42.	const subject = new Subject();
43.	
44.	const observer1 = new Observer('Observer 1');
45.	const observer2 = new Observer('Observer 2');
46.	
47.	subject.attach(observer1);
48.	subject.attach(observer2);
49.	
50.	subject.someBusinessLogic();
51.	// Output:
52.	// Subject: Doing something important...
53.	// Observer 1 has been notified.
54.	// Observer 2 has been notified.
55.	
56.	subject.detach(observer1);
57.	
58.	subject.someBusinessLogic();
59.	// Output:
60.	// Subject: Doing something important...
61.	// Observer 2 has been notified.
