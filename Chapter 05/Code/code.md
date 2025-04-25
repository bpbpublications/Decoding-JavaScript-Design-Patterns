# Synchronous JavaScript - Page 2
1.	// Synchronous code to fetch data and update UI
2.	function fetchDataAndRender() {
3.	  // Assume fetchData() is a synchronous function that fetches data from a server
4.	  const data = fetchData(); // This function might take some time to complete
5.	
6.	  // Assume renderUI() is a synchronous function that updates the UI with the fetched data
7.	  renderUI(data); // This function will not execute until fetchData() completes
8.	
9.	  console.log('UI updated with fetched data');
10.	}
11.	
12.	fetchDataAndRender();
13.	console.log('Fetching data and rendering UI...');


# Callback functions - Page 5
1.	// Asynchronous function with a callback
2.	function fetchData(callback) {
3.	  console.log("Fetching data...");
4.	  setTimeout(function() {
5.	    console.log("Data fetched!");
6.	    const data = { id: 1, name: "Example Data" };
7.	    callback(data);
8.	  }, 2000); // Simulating a delay of 2 seconds
9.	}
10.	
11.	// Callback function to handle the fetched data
12.	function processData(data) {
13.	  console.log("Processing data:", data);
14.	}
15.	
16.	// Calling the fetchData function with a callback
17.	fetchData(processData);
18.	
19.	console.log("Program continues to execute...");


# Callback Hell - Page 7
1.	function fetchUserData(userId, callback) {
2.	  // Simulated API call to fetch user data
3.	  setTimeout(function() {
4.	    const userData = { id: userId, name: "John Doe" };
5.	    console.log("User data fetched:", userData);
6.	    callback(userData);
7.	  }, 1000);
8.	}
9.	
10.	function fetchUserPosts(user, callback) {
11.	  // Simulated API call to fetch user posts
12.	  setTimeout(function() {
13.	    const posts = ["Post 1", "Post 2", "Post 3"];
14.	    console.log("User posts fetched:", posts);
15.	    callback(posts);
16.	  }, 1500);
17.	}
18.	
19.	function fetchPostComments(post, callback) {
20.	  // Simulated API call to fetch comments on a post
21.	  setTimeout(function() {
22.	    const comments = ["Comment 1", "Comment 2"];
23.	    console.log("Comments for post", post, ":", comments);
24.	    callback(comments);
25.	  }, 2000);
26.	}
27.	
28.	// Callback hell - nested callbacks
29.	fetchUserData(1, function(user) {
30.	  fetchUserPosts(user, function(posts) {
31.	    posts.forEach(function(post) {
32.	      fetchPostComments(post, function(comments) {
33.	        // Do something with comments
34.	      });
35.	    });
36.	  });
37.	});


# Promises - Page 10
1.	function fetchUserData(userId) {
2.	  return new Promise(function(resolve, reject) {
3.	    // Simulated API call to fetch user data
4.	    setTimeout(function() {
5.	      const userData = { id: userId, name: "John Doe" };
6.	      console.log("User data fetched:", userData);
7.	      resolve(userData);
8.	    }, 1000);
9.	  });
10.	}
11.	
12.	function fetchUserPosts(user) {
13.	  return new Promise(function(resolve, reject) {
14.	    // Simulated API call to fetch user posts
15.	    setTimeout(function() {
16.	      const posts = ["Post 1", "Post 2", "Post 3"];
17.	      console.log("User posts fetched:", posts);
18.	      resolve(posts);
19.	    }, 1500);
20.	  });
21.	}
22.	
23.	function fetchPostComments(post) {
24.	  return new Promise(function(resolve, reject) {
25.	    // Simulated API call to fetch comments on a post
26.	    setTimeout(function() {
27.	      const comments = ["Comment 1", "Comment 2"];
28.	      console.log("Comments for post", post, ":", comments);
29.	      resolve(comments);
30.	    }, 2000);
31.	  });
32.	}
33.	
34.	// Using promises to chain asynchronous operations without nesting
35.	fetchUserData(1)
36.	  .then(function(user) {
37.	    return fetchUserPosts(user);
38.	  })
39.	  .then(function(posts) {
40.	    return Promise.all(posts.map(fetchPostComments));
41.	  })
42.	  .then(function(commentsArray) {
43.	    // Do something with commentsArray
44.	  })
45.	  .catch(function(error) {
46.	    console.error("Error:", error);
47.	  });


# Async/await - Page 12
1.	function fetchUserData(userId) {
2.	  return new Promise(function(resolve, reject) {
3.	    // Simulated API call to fetch user data
4.	    setTimeout(function() {
5.	      const userData = { id: userId, name: "John Doe" };
6.	      console.log("User data fetched:", userData);
7.	      resolve(userData);
8.	    }, 1000);
9.	  });
10.	}
11.	
12.	function fetchUserPosts(user) {
13.	  return new Promise(function(resolve, reject) {
14.	    // Simulated API call to fetch user posts
15.	    setTimeout(function() {
16.	      const posts = ["Post 1", "Post 2", "Post 3"];
17.	      console.log("User posts fetched:", posts);
18.	      resolve(posts);
19.	    }, 1500);
20.	  });
21.	}
22.	
23.	function fetchPostComments(post) {
24.	  return new Promise(function(resolve, reject) {
25.	    // Simulated API call to fetch comments on a post
26.	    setTimeout(function() {
27.	      const comments = ["Comment 1", "Comment 2"];
28.	      console.log("Comments for post", post, ":", comments);
29.	      resolve(comments);
30.	    }, 2000);
31.	  });
32.	}
33.	
34.	async function fetchData() {
35.	  try {
36.	    const user = await fetchUserData(1);
37.	    const posts = await fetchUserPosts(user);
38.	    const commentsArray = await Promise.all(posts.map(fetchPostComments));
39.	    // Do something with commentsArray
40.	  } catch (error) {
41.	    console.error("Error:", error);
42.	  }
43.	}
44.	
45.	// Call the fetchData function
46.	fetchData();


# Generators - Page 14
1.	function* generatorFunction() {
2.	    yield 1;
3.	    yield 2;
4.	    yield 3;
5.	}
6.	
7.	const generator = generatorFunction();
8.	console.log(generator.next()); // { value: 1, done: false }
9.	console.log(generator.next()); // { value: 2, done: false }
10.	console.log(generator.next()); // { value: 3, done: false }
11.	console.log(generator.next()); // { value: undefined, done: true }


# Event Handling in HTML - Page 17
1.	<!DOCTYPE html>
2.	<html lang="en">
3.	<head>
4.	    <meta charset="UTF-8">
5.	    <meta name="viewport" content="width=device-width, initial-scale=1.0">
6.	    <title>Event Handling Example</title>
7.	</head>
8.	<body>
9.	    <button id="myButton">Click Me!</button>
10.	
11.	    <script>
12.	        // Get a reference to the button element
13.	        var button = document.getElementById('myButton');
14.	
15.	        // Add an event listener to the button
16.	        button.addEventListener('click', function() {
17.	            // Code to run when the button is clicked
18.	            alert('Button clicked!');
19.	        });
20.	    </script>
21.	</body>
22.	</html>


# Event Propogation - Page 19
1.	console.log("Start");
2.	
3.	setTimeout(function() {
4.	    console.log("Inside setTimeout");
5.	}, 0);
6.	
7.	console.log("End");


# Step 1 of Throttling - Page 21
unction throttle(func, delay) {
2.	  let lastCall = 0; // Initialize a variable to store the timestamp of the last function call
3.	
4.	  return function(...args) {
5.	    const now = new Date().getTime(); // Get the current timestamp
6.	    if (now - lastCall >= delay) { // Check if enough time has elapsed since the last call
7.	      func(...args); // Call the original function with the provided arguments
8.	      lastCall = now; // Update the timestamp of the last call
9.	    }
10.	  };
11.	}


# Step 2 of Throttling - Page 21
2.	function makeNetworkRequest(data) {
3.	  console.log(`Making network request with data: ${data}`);
4.	}
5.	
6.	// Create a throttled version of the network request function with a delay of 1000 milliseconds (1 second)
7.	const throttledNetworkRequest = throttle(makeNetworkRequest, 1000);


# Step 3 of Throttling - Page 22
1.	document.getElementById('searchInput').addEventListener('input', function(event) {
2.	  const query = event.target.value;
3.	  throttledNetworkRequest(query);
4.	});


# Step 1 of Debouncing - Page 25
1.	function debounce(func, delay) {
2.	  let timeoutId;
3.	
4.	  return function(...args) {
5.	    clearTimeout(timeoutId); // Clear any existing timeout
6.	    timeoutId = setTimeout(() => {
7.	      func(...args); // Call the original function after the delay
8.	    }, delay);
9.	  };
10.	}


# Step 2 of Debouncing - Page 25
1.	// Function to simulate making a network request
2.	function makeNetworkRequest(data) {
3.	  console.log(`Making network request with data: ${data}`);
4.	}
5.	
6.	// Create a debounced version of the network request function with a delay of 1000 milliseconds (1 second)
7.	const debouncedNetworkRequest = debounce(makeNetworkRequest, 1000);


# Step 3 of Debouncing - Page 26
1.	// Event listener for search input
2.	document.getElementById('searchInput').addEventListener('input', function(event) {
3.	  const query = event.target.value;
4.	  debouncedNetworkRequest(query);
5.	});


# Alternate method of calling debouncing - Page 26
1.	// Example usage of the debounced function
2.	debouncedNetworkRequest(); // This call will be ignored because no subsequent call is made
3.	debouncedNetworkRequest(); // This call will also be ignored
4.	// Wait for 500 milliseconds
5.	setTimeout(() => {
6.	  debouncedNetworkRequest(); // This call will execute myFunction after the debounce delay has elapsed
7.	}, 500);


# Web Workers: Problems with async requests - Page 29
1.	// Function to perform intensive calculations (e.g., factorial calculation)
2.	function calculateFactorial(n) {
3.	    let result = 1;
4.	    for (let i = 1; i <= n; i++) {
5.	        result *= i;
6.	    }
7.	    return result;
8.	}
9.	
10.	// Function to handle button click event
11.	document.getElementById('calculateButton').addEventListener('click', function() {
12.	    const input = document.getElementById('numberInput').value;
13.	    const result = calculateFactorial(parseInt(input));
14.	    document.getElementById('result').innerText = 'Factorial: ' + result;
15.	});


# Step 1 of web workers - Page 30
1.	// worker.js
2.	
3.	// Function to perform intensive calculations (e.g., factorial calculation)
4.	function calculateFactorial(n) {
5.	    let result = 1;
6.	    for (let i = 1; i <= n; i++) {
7.	        result *= i;
8.	    }
9.	    return result;
10.	}
11.	
12.	// Event listener for receiving messages from the main thread
13.	self.addEventListener('message', function(event) {
14.	    const input = event.data;
15.	    const result = calculateFactorial(input);
16.	    // Send the result back to the main thread
17.	    self.postMessage(result);
18.	});


# Step 2 of web workers - Page 31
1.	// main.js
2.	
3.	// Create a new Web Worker instance
4.	const worker = new Worker('worker.js');


# Step 3 of web workers - Page 32
1.	// main.js
2.	
3.	// Send data to the Web Worker
4.	const inputNumber = 10; // Example input
5.	worker.postMessage(inputNumber);


# Step 4 of web workers - Page 32
1.	// main.js
2.	
3.	// Receive results from the Web Worker
4.	worker.addEventListener('message', function(event) {
5.	    const result = event.data;
6.	    console.log('Result received from Web Worker:', result);
7.	    // Update the UI or perform further actions with the result
8.	});


# Step 5 of web workers - Page 33
1.	// Terminate the Web Worker (optional)
2.	worker.terminate();


# Step 6 of web workers - Page 33
1.	// Error handling for the Web Worker
2.	worker.addEventListener('error', function(event) {
3.	    console.error('Error in Web Worker:', event.message);
4.	});


# Problematic code without batch processing - Page 37
1.	// Problematic code without batch processing
2.	async function fetchDataFromAPI(urls) {
3.	    let results = [];
4.	    for (let url of urls) {
5.	        try {
6.	            let response = await fetch(url);
7.	            let data = await response.json();
8.	            results.push(data);
9.	        } catch (error) {
10.	            console.error(`Error fetching data from ${url}: ${error}`);
11.	        }
12.	    }
13.	    return results;
14.	}
15.	
16.	let urls = ['https://api.example.com/data/1', 'https://api.example.com/data/2', 'https://api.example.com/data/3'];
17.	
18.	fetchDataFromAPI(urls)
19.	    .then(results => {
20.	        console.log(results);
21.	    })
22.	    .catch(error => {
23.	        console.error(error);
24.	    });


# Step 1 of batch processing - Page 38
1.	async function processBatch(urls, batchSize) {
2.	    let results = [];
3.	    for (let i = 0; i < urls.length; i += batchSize) {
4.	        let batchUrls = urls.slice(i, i + batchSize);
5.	        let batchResults = await Promise.all(batchUrls.map(url => fetchData(url)));
6.	        results.push(...batchResults);
7.	    }
8.	    return results;
9.	}


# Step 2 of batch processing - Page 38
1.	async function fetchData(url) {
2.	    try {
3.	        let response = await fetch(url);
4.	        return await response.json();
5.	    } catch (error) {
6.	        console.error(`Error fetching data from ${url}: ${error}`);
7.	        return null;
8.	    }
9.	}


# Step 3 of batch processing - Page 39
1.	let urls = [
2.	    'https://api.example.com/data/1',
3.	    'https://api.example.com/data/2',
4.	    'https://api.example.com/data/3',
5.	    // Add more URLs as needed
6.	];


# Step 4 of batch processing - Page 40
1.	const batchSize = 2; // Specify the batch size
2.	processBatch(urls, batchSize)
3.	    .then(results => {
4.	        console.log(results);
5.	    })
6.	    .catch(error => {
7.	        console.error(error);
8.	    });


# Nested promises bad example - Page 43
1.	// Bad approach with nested promises
2.	asyncFunction1()
3.	  .then(result1 => {
4.	    return asyncFunction2(result1)
5.	      .then(result2 => {
6.	        return asyncFunction3(result2);
7.	      });
8.	  })
9.	  .catch(error => {
10.	    console.error('Error:', error);
11.	  });
12.	
13.	// Improved approach with promise chaining
14.	asyncFunction1()
15.	  .then(result1 => asyncFunction2(result1))
16.	  .then(result2 => asyncFunction3(result2))
17.	  .catch(error => {
18.	    console.error('Error:', error);
19.	  });


# Promise Chaining - Page 43
1.	// Chaining promises using async/await
2.	async function sequentialAsyncOperations() {
3.	  try {
4.	    const result1 = await asyncFunction1();
5.	    const result2 = await asyncFunction2(result1);
6.	    const result3 = await asyncFunction3(result2);
7.	    return result3;
8.	  } catch (error) {
9.	    console.error('Error:', error);
10.	  }
11.	}
12.	
13.	sequentialAsyncOperations();


# Error Handling - Page 44
1.	// Error handling with catch block
2.	asyncFunction()
3.	  .then(result => {
4.	    // Handle result
5.	  })
6.	  .catch(error => {
7.	    console.error('Error:', error);
8.	  });


# Parallel Promises - Page 44
1.	// Parallel promises using Promise.all()
2.	Promise.all([asyncFunction1(), asyncFunction2(), asyncFunction3()])
3.	  .then(results => {
4.	    // Handle results array
5.	  })
6.	  .catch(error => {
7.	    console.error('Error:', error);
8.	  });


# Promise reuse - Page 45
1.	// Reusing the same promise instance
2.	const sharedPromise = asyncFunction();
3.	
4.	sharedPromise.then(result => {
5.	  // Handle result
6.	});
7.	
8.	sharedPromise.catch(error => {
9.	  console.error('Error:', error);
10.	});


# Memory Management - Page 45
1.	// Ensure proper disposal of promises
2.	let promise = asyncFunction();
3.	
4.	// After using the promise, set it to null or undefined
5.	promise = null;


# Performance considerations - Page 46
1.	// Minimize unnecessary asynchronous operations
2.	const data = fetchData(); // Synchronous operation
3.	
4.	// Optimize asynchronous code for execution speed
5.	asyncFunction()
6.	  .then(result => {
7.	    // Handle result
8.	  })
9.	  .catch(error => {
10.	    console.error('Error:', error);
11.	  });
