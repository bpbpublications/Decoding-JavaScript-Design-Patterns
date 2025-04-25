# Components in Vue - Page 4
1.	<!-- BlogPost.vue -->
2.	<template>
3.	  <div class="blog-post">
4.	    <h3>{{ title }}</h3>
5.	    <p>{{ content }}</p>
6.	  </div>
7.	</template>
8.	
9.	<script>
10.	export default {
11.	  props: {
12.	    title: String, // Title of the blog post
13.	    content: String // Content of the blog post
14.	  }
15.	};
16.	</script>
17.	
18.	<style scoped>
19.	.blog-post {
20.	  margin-bottom: 20px;
21.	  border: 1px solid #ccc;
22.	  padding: 10px;
23.	}
24.	</style>


# Using the component in Vue - Page 4
1.	<!-- Blog.vue -->
2.	<template>
3.	  <div>
4.	    <h2>My Blog</h2>
5.	    <BlogPost v-for="(post, index) in posts" :key="index" :title="post.title" :content="post.content" />
6.	  </div>
7.	</template>
8.	
9.	<script>
10.	import BlogPost from './BlogPost.vue'; // Import BlogPost component
11.	
12.	export default {
13.	  components: {
14.	    BlogPost // Register BlogPost component
15.	  },
16.	  data() {
17.	    return {
18.	      posts: [ // Array of blog posts
19.	        { title: 'First Post', content: 'This is the content of my first post.' },
20.	        { title: 'Second Post', content: 'This is the content of my second post.' },
21.	        { title: 'Third Post', content: 'This is the content of my third post.' }
22.	      ]
23.	    };
24.	  }
25.	};
26.	</script>


# State management - store - Page 6
1.	// store/index.js
2.	
3.	import Vue from 'vue';
4.	import Vuex from 'vuex';
5.	
6.	Vue.use(Vuex);
7.	
8.	export default new Vuex.Store({
9.	  state: {
10.	    count: 0
11.	  },
12.	  mutations: {
13.	    increment(state) {
14.	      state.count++;
15.	    },
16.	    decrement(state) {
17.	      state.count--;
18.	    }
19.	  },
20.	  actions: {
21.	    increment({ commit }) {
22.	      commit('increment');
23.	    },
24.	    decrement({ commit }) {
25.	      commit('decrement');
26.	    }
27.	  },
28.	  getters: {
29.	    getCount(state) {
30.	      return state.count;
31.	    }
32.	  }
33.	});


# State management - store - Page 7
1.	<!-- Counter.vue -->
2.	
3.	<template>
4.	  <div>
5.	    <h2>Counter: {{ count }}</h2>
6.	    <button @click="increment">Increment</button>
7.	    <button @click="decrement">Decrement</button>
8.	  </div>
9.	</template>
10.	
11.	<script>
12.	export default {
13.	  computed: {
14.	    count() {
15.	      return this.$store.getters.getCount;
16.	    }
17.	  },
18.	  methods: {
19.	    increment() {
20.	      this.$store.dispatch('increment');
21.	    },
22.	    decrement() {
23.	      this.$store.dispatch('decrement');
24.	    }
25.	  }
26.	};
27.	</script>


# Custom directive approach - Page 9
1.	// Custom directive definition
2.	Vue.directive('validate', {
3.	  bind(el, binding, vnode) {
4.	    // Validation logic here
5.	    el.addEventListener('blur', () => {
6.	      // Perform validation
7.	      if (!isValid) {
8.	        // Show error message
9.	        vnode.context.$emit('show-error', errorMessage);
10.	      }
11.	    });
12.	  }
13.	});


# Using custom directives - Page 10
1.	<input type="text" v-validate="validateRules">


# Validation mixin - Page 11
1.	// Validation mixin
2.	const ValidationMixin = {
3.	  methods: {
4.	    validate() {
5.	      // Validation logic
6.	      if (!isValid) {
7.	        this.showError(errorMessage);
8.	      }
9.	    },
10.	    showError(message) {
11.	      // Show error message
12.	    }
13.	  }
14.	};


# Using validation mixin - Page 11
1.	import ValidationMixin from './ValidationMixin';
2.	
3.	export default {
4.	  mixins: [ValidationMixin],
5.	  // Component definition
6.	}

1.	<input type="text" @blur="validate">


# Difference between computed and watch - Page 12
1.	// Vue component
2.	export default {
3.	  data() {
4.	    return {
5.	      firstName: 'John',
6.	      lastName: 'Doe',
7.	    };
8.	  },
9.	  computed: {
10.	    fullName() {
11.	      return `${this.firstName} ${this.lastName}`;
12.	    },
13.	  },
14.	  watch: {
15.	    firstName(newValue, oldValue) {
16.	      console.log(`firstName changed from ${oldValue} to ${newValue}`);
17.	    },
18.	    lastName(newValue, oldValue) {
19.	      console.log(`lastName changed from ${oldValue} to ${newValue}`);
20.	    },
21.	  },
22.	};


# Scoped styles - Page 14 
1.	<template>
2.	  <div class="example">
3.	    <h1>Hello, world!</h1>
4.	  </div>
5.	</template>
6.	
7.	<script>
8.	export default {
9.	  name: 'ExampleComponent'
10.	}
11.	</script>
12.	
13.	<style scoped>
14.	/* These styles will only apply to the h1 element within this component */
15.	h1 {
16.	  color: red;
17.	}
18.	</style>


# Dynamic component loading - bind directive - Page 15
1.	<component v-bind:is="componentName"></component>


# Dynamic component loading - dynamic import - Page 16
1.	const MyComponent = () => import('./MyComponent.vue');


# Dynamic component loading - async components - Page 16
1.	Vue.component('async-component', () => import('./AsyncComponent.vue'));

1.	const router = new VueRouter({
2.	  routes: [
3.	    { path: '/home', component: () => import('./Home.vue') },
4.	    { path: '/about', component: () => import('./About.vue') },
5.	    { path: '/contact', component: () => import('./Contact.vue') }
6.	  ]
7.	});


# Singleton pattern - Page 16
1.	// Singleton pattern with a global variable
2.	const EventBus = new Vue();
3.	
4.	// Usage
5.	EventBus.$emit('event', data);


# Factory pattern - Page 16
1.	function createComponent(type) {
2.	  if (type === 'button') {
3.	    return Vue.component('my-button', {
4.	      // button component options
5.	    });
6.	  } else if (type === 'input') {
7.	    return Vue.component('my-input', {
8.	      // input component options
9.	    });
10.	  }
11.	}
12.	
13.	// Usage
14.	const ButtonComponent = createComponent('button');




