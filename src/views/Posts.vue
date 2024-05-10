<template>
  <div class="container">
    <header class="header">
      <nav class="navbar">
        <router-link to="/" class="active">Todo App</router-link>
        <router-link to="/posts">Posts</router-link>
      </nav>
    </header>
    <div class="posts" id="posts">
      <h1>Postingan user</h1>
      <h2>{{ selectedUserName }}</h2> 
      <select v-model="selectedUser" class="selectModel" @change="fetchPosts">
        <option v-for="user in users" :value="user.id" :key="user.id">{{ user.name }}</option>
      </select>

      <div v-if="isLoading">
        <span class="loading-message">Loading posts...</span>
      </div>

      <ul v-if="!isLoading"> 
        <li v-for="post in posts" :key="post.id">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';

const isLoading = ref(false); 
const selectedUser = ref(null);
const selectedUserName = ref('');
const users = ref([]);
const posts = ref([]);

const fetchUser = async () => {
  try {
    isLoading.value = true; 
    const response = await fetch('https://jsonplaceholder.typicode.com/users');
    const data = await response.json();
    users.value = data;
  } catch (error) {
    console.error('Error fetching users:', error);
  } finally {
    isLoading.value = false; 
  }
};

const fetchPosts = async () => {
  if (!selectedUser.value) return;
  isLoading.value = true;
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`);
    const data = await response.json();
    posts.value = data;
  } catch (error) {
    console.error('Error fetching posts:', error);
  } finally {
    isLoading.value = false; 
  }
};

onMounted(() => {
  fetchUser();
});

watch(selectedUser, () => {
  posts.value = []; 
  fetchPosts();
  const selectedUserObject = users.value.find(user => user.id === selectedUser.value);
  selectedUserName.value = selectedUserObject ? selectedUserObject.name : '';
}, { immediate: true }); 
</script>

<style scoped>
/* Global styles */
@media (prefers-color-scheme: dark) {
  .todo-app h2 ul li {
    color: #000; 
  }
}

/* Section styles */
section {
  min-height: 100vh;
  padding: 10rem 9% 2rem; 
}

/* Navbar styles */

.navbar {
  display: flex;
  justify-content: center; 
  align-items: center; 
  width: 100%; 
}

.navbar a {
  font-size: 1.5rem;
  color: white;
  margin: 0 3rem; 
  font-weight: 700;
  text-decoration: none; 
}

/* Header styles */
.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  padding: 2rem 0%;
  z-index: 1000; 
  background: linear-gradient(90deg, hsla(197, 30%, 54%, 1) 7%, hsla(275, 19%, 88%, 1) 100%);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* Container styles */
.container {
  width: 100%;
  min-height: 100vh;
  background: -webkit-linear-gradient(90deg, hsla(120, 6%, 90%, 1) 0%, hsla(228, 75%, 16%, 1) 100%);
  filter: progid: DXImageTransform.Microsoft.gradient( startColorstr="#E4E7E4", endColorstr="#0A1647", GradientType=1 );
  padding: 10px;
  margin-top: -10px;
  margin-left: -10px;
  margin-bottom: -10px;
}

.posts {
    width: 100%;
    max-width: 1200px;
    background: #fff;
    margin: 20vh auto;
    padding: 40px 30px 70px;
    border-radius: 10px;
  }


.loading-message {
  display: flex;
  justify-content:center;
  text-align: center;
  margin-bottom: 10px;
}

.selectModel {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  text-align:center;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #fff;
  cursor: pointer;
}

.selectModel:focus {
  outline: none;
  border-color: #4CAF50; 
}

.posts ul {
  list-style-type: none; 
  padding-left: 0; 
}

.posts li {
  margin-bottom: 20px; 
}

.posts h3, .posts p {
  text-align: left; 
}
</style>