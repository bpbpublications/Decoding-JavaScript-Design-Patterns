# Class-based components - Page 4
1.	import React, { Component } from 'react';
2.	
3.	class Post extends Component {
4.	  constructor(props) {
5.	    super(props);
6.	    this.state = {
7.	      isLiked: false, // Initial like state
8.	    };
9.	  }
10.	
11.	  handleLike = () => {
12.	    this.setState(prevState => ({
13.	      isLiked: !prevState.isLiked
14.	    }));
15.	  }
16.	
17.	  render() {
18.	    const { profilePic, handle, caption, content } = this.props;
19.	    const likeIcon = this.state.isLiked ? 'liked.svg' : 'unliked.svg';
20.	
21.	    return (
22.	      <div className="post">
23.	        <div className="post-header">
24.	          <img src={profilePic} alt="Profile Pic" className="profile-pic" />
25.	          <span className="handle">{handle}</span>
26.	        </div>
27.	        <div className="post-content">
28.	          <p className="caption">{caption}</p>
29.	          {content && <img src={content} alt="Post Content" className="post-image" />}
30.	          <button className="like-button" onClick={this.handleLike}>
31.	            <img src={likeIcon} alt="Like Button" />
32.	          </button>
33.	        </div>
34.	      </div>
35.	    );
36.	  }
37.	}
38.	
39.	export default Post;


# Functional components - Page 6
1.	import React, { useState } from 'react';
2.	
3.	const Story = ({ profilePic, handle }) => {
4.	  const [isViewing, setIsViewing] = useState(false); // Initial viewing state
5.	
6.	  const handleView = () => {
7.	    setIsViewing(prevIsViewing => !prevIsViewing);
8.	  };
9.	
10.	  return (
11.	    <div className="story">
12.	      <img
13.	        src={profilePic}
14.	        alt="Profile Pic"
15.	        className={isViewing ? 'story-profile-pic-active' : 'story-profile-pic'}
16.	        onClick={handleView}
17.	      />
18.	      {isViewing && (
19.	        <div className="story-content">
20.	          <span className="story-handle">{handle}</span>
21.	        </div>
22.	      )}
23.	    </div>
24.	  );
25.	};
26.	
27.	export default Story;


# Props (example 1) - Page 8
1.	<Post 
2.	     profilePic="bpb-profile.png" 
3.	     handle="bpbPublications" 
4.	     caption="The latest in technology for 2024"
5.	     content="latest-in-tech-2024.png"
6.	/>


# Props (example 2) - Page 8
1.	<Story 
2.	     profilePic="bpb-profile.png" 
3.	     handle="bpbPublications" 
4.	/>


# useRef hook - Page 11
1.	import React, { useRef } from 'react';
2.	
3.	function MyComponent() {
4.	  // Create a ref object
5.	  const inputRef = useRef(null);
6.	
7.	  // Function to focus the input field
8.	  const focusInput = () => {
9.	    inputRef.current.focus();
10.	  };
11.	
12.	  return (
13.	    <div>
14.	      {/* Assign the ref to the input element */}
15.	      <input ref={inputRef} type="text" />
16.	      <button onClick={focusInput}>Focus Input</button>
17.	    </div>
18.	  );
19.	}
20.	
21.	export default MyComponent;


# useEffect hook - Page 13
1.	import React, { useEffect, useState } from 'react';
2.	
3.	function MyComponent() {
4.	  const [count, setCount] = useState(0);
5.	
6.	  useEffect(() => {
7.	    // Update the document title with the current count
8.	    document.title = `Count: ${count}`;
9.	  });
10.	
11.	  return (
12.	    <div>
13.	      <p>Current count: {count}</p>
14.	      <button onClick={() => setCount(count + 1)}>Increment</button>
15.	    </div>
16.	  );
17.	}
18.	
19.	export default MyComponent;


# Higher Order Components - Page 15
1.	import React from 'react';
2.	import { Redirect } from 'react-router-dom';
3.	
4.	const withAuth = (WrappedComponent) => {
5.	  const AuthenticatedComponent = (props) => {
6.	    const isAuthenticated = checkIfUserIsAuthenticated(); // Function to check if user is authenticated
7.	
8.	    if (isAuthenticated) {
9.	      return <WrappedComponent {...props} />;
10.	    } else {
11.	      return <Redirect to="/login" />;
12.	    }
13.	  };
14.	
15.	  return AuthenticatedComponent;
16.	};
17.	
18.	export default withAuth;


# HOC used by a component - Page 16
1.	import React from 'react';
2.	import withAuth from './withAuth';
3.	
4.	const Dashboard = () => {
5.	  return (
6.	    <div>
7.	      <h1>Welcome to the Dashboard</h1>
8.	      {/* Dashboard content */}
9.	    </div>
10.	  );
11.	};
12.	
13.	export default withAuth(Dashboard);


# Container components - Page 19
1.	// Container Component
2.	import React, { Component } from 'react';
3.	import { connect } from 'react-redux';
4.	import { fetchData } from './actions';
5.	
6.	class UserContainer extends Component {
7.	  componentDidMount() {
8.	    this.props.fetchData();
9.	  }
10.	
11.	  render() {
12.	    const { isLoading, users, error } = this.props;
13.	
14.	    return (
15.	      <div>
16.	        {isLoading ? (
17.	          <p>Loading...</p>
18.	        ) : error ? (
19.	          <p>Error: {error.message}</p>
20.	        ) : (
21.	          <UserList users={users} />
22.	        )}
23.	      </div>
24.	    );
25.	  }
26.	}
27.	
28.	const mapStateToProps = state => ({
29.	  isLoading: state.isLoading,
30.	  users: state.users,
31.	  error: state.error
32.	});
33.	
34.	const mapDispatchToProps = dispatch => ({
35.	  fetchData: () => dispatch(fetchData())
36.	});
37.	
38.	export default connect(mapStateToProps, mapDispatchToProps)(UserContainer);


# Presentation component - Page 20
1.	// Presentation Component
2.	import React from 'react';
3.	
4.	const UserList = ({ users }) => (
5.	  <ul>
6.	    {users.map(user => (
7.	      <li key={user.id}>{user.name}</li>
8.	    ))}
9.	  </ul>
10.	);
11.	
12.	export default UserList;


# Render Props - Page 22
1.	// Counter.js
2.	import React, { useState } from 'react';
3.	
4.	const Counter = ({ render }) => {
5.	  const [count, setCount] = useState(0);
6.	
7.	  const incrementCount = () => {
8.	    setCount(count + 1);
9.	  };
10.	
11.	  return (
12.	    <div>
13.	      {render(count, incrementCount)}
14.	    </div>
15.	  );
16.	};
17.	
18.	export default Counter;


# Create button component for render props - Page 23
1.	// Button.js
2.	import React from 'react';
3.	
4.	const Button = ({ onClick, label }) => {
5.	  return <button onClick={onClick}>{label}</button>;
6.	};
7.	
8.	export default Button;


# Adding a wrapper for render props - Page 23
1.	import React from 'react';
2.	import Counter from './Counter';
3.	import Button from './Button';
4.	
5.	const App = () => {
6.	  return (
7.	    <div>
8.	      <Counter
9.	        render={(count, incrementCount) => (
10.	          <Button onClick={incrementCount} label={`Clicked ${count} times`} />
11.	        )}
12.	      />
13.	    </div>
14.	  );
15.	};
16.	
17.	export default App;


# Step 1 of Context API - Page 25
1.	const MyContext = React.createContext();


# Step 2 of Context API - Page 26
1.	<MyContext.Provider value={/* some value */}>
2.	  {/* Your component tree */}
3.	</MyContext.Provider>


# Step 3 of Context API - Page 26
1.	// using Consumer wrapper
2.	<MyContext.Consumer>
3.	  {value => /* render something based on the context value */}
4.	</MyContext.Consumer>
5.	
6.	// useContext hook
7.	import React, { useContext } from 'react';
8.	
9.	function MyComponent() {
10.	  const value = useContext(MyContext);
11.	  // Use the context value here
12.	}


# Code Example of Context API - Page 26
1.	// ThemeContext.js
2.	import React from 'react';
3.	
4.	const ThemeContext = React.createContext('light');
5.	
6.	export default ThemeContext;
7.	
8.	// App.js
9.	import React from 'react';
10.	import ThemeContext from './ThemeContext';
11.	import Toolbar from './Toolbar';
12.	
13.	class App extends React.Component {
14.	  render() {
15.	    return (
16.	      <ThemeContext.Provider value="dark">
17.	        <Toolbar />
18.	      </ThemeContext.Provider>
19.	    );
20.	  }
21.	}
22.	
23.	// Toolbar.js
24.	import React from 'react';
25.	import ThemeContext from './ThemeContext';
26.	
27.	function Toolbar() {
28.	  return (
29.	    <div>
30.	      <ThemedButton />
31.	    </div>
32.	  );
33.	}
34.	
35.	// ThemedButton.js
36.	import React from 'react';
37.	import ThemeContext from './ThemeContext';
38.	
39.	function ThemedButton() {
40.	  const theme = useContext(ThemeContext);
41.	  return <button style={{ background: theme }}>Themed Button</button>;
42.	}


# Creating Header.js for Module pattern - Page 30
1.	// Header.js
2.	import React from 'react';
3.	
4.	const Header = () => {
5.	  return (
6.	    <header>
7.	      <h1>My Blog</h1>
8.	    </header>
9.	  );
10.	};
11.	
12.	export default Header;


# Creating Postlist.js for Module pattern - Page 30
1.	// PostList.js
2.	import React from 'react';
3.	
4.	const PostList = ({ posts }) => {
5.	  return (
6.	    <div>
7.	      <h2>Latest Posts</h2>
8.	      <ul>
9.	        {posts.map(post => (
10.	          <li key={post.id}>
11.	            <h3>{post.title}</h3>
12.	            <p>{post.body}</p>
13.	          </li>
14.	        ))}
15.	      </ul>
16.	    </div>
17.	  );
18.	};
19.	
20.	export default PostList;


# Creating Footer.js for Module pattern - Page 31
1.	// Footer.js
2.	import React from 'react';
3.	
4.	const Footer = () => {
5.	  return (
6.	    <footer>
7.	      <p>&copy; 2024 My Blog</p>
8.	    </footer>
9.	  );
10.	};
11.	
12.	export default Footer;


# App.js for Module pattern - Page 31
1.	// App.js
2.	import React from 'react';
3.	import Header from './Header';
4.	import PostList from './PostList';
5.	import Footer from './Footer';
6.	
7.	const App = () => {
8.	  const posts = [
9.	    { id: 1, title: 'First Post', body: 'This is the content of the first post.' },
10.	    { id: 2, title: 'Second Post', body: 'This is the content of the second post.' },
11.	    { id: 3, title: 'Third Post', body: 'This is the content of the third post.' },
12.	  ];
13.	
14.	  return (
15.	    <div>
16.	      <Header />
17.	      <PostList posts={posts} />
18.	      <Footer />
19.	    </div>
20.	  );
21.	};
22.	
23.	export default App;


# Factory design pattern - Page 32
1.	import React from 'react';
2.	
3.	// Define different types of components
4.	const Button = ({ onClick, children }) => (
5.	  <button onClick={onClick}>{children}</button>
6.	);
7.	
8.	const Input = ({ onChange, value }) => (
9.	  <input type="text" onChange={onChange} value={value} />
10.	);
11.	
12.	// Factory function to create components based on type
13.	const ComponentFactory = {
14.	  createComponent: (type, props) => {
15.	    switch (type) {
16.	      case 'button':
17.	        return <Button {...props} />;
18.	      case 'input':
19.	        return <Input {...props} />;
20.	      default:
21.	        throw new Error('Invalid component type');
22.	    }
23.	  },
24.	};
25.	
26.	// Example usage of the factory function
27.	const MyComponent = () => {
28.	  const buttonComponent = ComponentFactory.createComponent('button', {
29.	    onClick: () => alert('Button clicked!'),
30.	    children: 'Click me',
31.	  });
32.	
33.	  const inputComponent = ComponentFactory.createComponent('input', {
34.	    onChange: (e) => console.log(e.target.value),
35.	    value: '',
36.	  });
37.	
38.	  return (
39.	    <div>
40.	      {buttonComponent}
41.	      {inputComponent}
42.	    </div>
43.	  );
44.	};
45.	
46.	export default MyComponent;


# Observer Pattern - Page 34
1.	import React, { useState, useEffect } from 'react';
2.	
3.	// Subject component
4.	const SubjectComponent = () => {
5.	  const [subjectState, setSubjectState] = useState('Initial state');
6.	  const [observers, setObservers] = useState([]);
7.	
8.	  // Function to update subject state
9.	  const updateState = () => {
10.	    setSubjectState('New state');
11.	  };
12.	
13.	  useEffect(() => {
14.	    // Notify observers when state changes
15.	    notifyObservers();
16.	  }, [subjectState]);
17.	
18.	  // Function to register observers
19.	  const addObserver = (observer) => {
20.	    setObservers([...observers, observer]);
21.	  };
22.	
23.	  // Function to notify observers
24.	  const notifyObservers = () => {
25.	    observers.forEach((observer) => observer.update(subjectState));
26.	  };
27.	
28.	  return (
29.	    <div>
30.	      <h2>Subject Component</h2>
31.	      <p>State: {subjectState}</p>
32.	      <button onClick={updateState}>Change State</button>
33.	    </div>
34.	  );
35.	};
36.	
37.	// Observer component
38.	const ObserverComponent = ({ subject }) => {
39.	  const [observerState, setObserverState] = useState('');
40.	
41.	  // Function to update observer state
42.	  const update = (newState) => {
43.	    setObserverState(newState);
44.	  };
45.	
46.	  useEffect(() => {
47.	    // Add itself as an observer to the SubjectComponent
48.	    subject.addObserver({ update });
49.	    return () => {
50.	      // Cleanup: Remove itself as an observer
51.	      subject.removeObserver({ update });
52.	    };
53.	  }, [subject]);
54.	
55.	  return (
56.	    <div>
57.	      <h2>Observer Component</h2>
58.	      <p>State: {observerState}</p>
59.	    </div>
60.	  );
61.	};
62.	
63.	// App component
64.	const App = () => {
65.	  return (
66.	    <div>
67.	      <SubjectComponent />
68.	      <ObserverComponent subject={SubjectComponent} />
69.	    </div>
70.	  );
71.	};
72.	
73.	export default App;


# withBorder.js for Decorator pattern - Page 36
1.	// withBorder.js
2.	import React from 'react';
3.	
4.	const withBorder = (WrappedComponent) => {
5.	  return class WithBorder extends React.Component {
6.	    render() {
7.	      return (
8.	        <div style={{ border: '1px solid red' }}>
9.	          <WrappedComponent {...this.props} />
10.	        </div>
11.	      );
12.	    }
13.	  };
14.	};
15.	
16.	export default withBorder;


# MyComponent.js for Decorator pattern - Page 36
1.	// MyComponent.js
2.	import React from 'react';
3.	import withBorder from './withBorder';
4.	
5.	class MyComponent extends React.Component {
6.	  render() {
7.	    return <div>Hello, World!</div>;
8.	  }
9.	}
10.	
11.	export default withBorder(MyComponent);
