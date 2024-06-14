<template>
  <div class="container">
    <div class="todo-app" id="todo-app">
      <h2>To-Do List Saya</h2>
      <div class="row">
        <input type="text" v-model="newTask" placeholder="Masukkan Agenda">
        <button @click="addTask">Tambah</button>
      </div>
      <div class="filters">
        <button @click="hideCompleted = !hideCompleted" class="button-74">
          {{ hideCompleted ? 'View Complete' : 'Hide Complete' }}
        </button>
      </div>
      <ul class="lists">
        <li
          v-for="(task, index) in filteredTodos"
          :key="task.id"
          :class="{ checked: task.checked, editing: task.editing }"
          @click="toggleTask(task)"
        >
          <template v-if="!task.editing">
            {{ task.text }}
            <span @click.stop="removeTask(task)" class="removebutton">&#xd7;</span>
            <span @click.stop="editTask(task)" class="editbutton">&#x270E;</span>
          </template>
          <template v-else>
            <input type="text" v-model="task.text" @keyup.enter="updateTask(task)">
            <span @click.stop="cancelEdit(task)" class="editbutton">&#x2716;</span>
          </template>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { useTodoStore } from '../store/todoStore'
import { storeToRefs } from 'pinia'

const todoStore = useTodoStore()
const { tasks, newTask, hideCompleted, filteredTodos } = storeToRefs(todoStore)
const { addTask, toggleTask, removeTask, editTask, updateTask, cancelEdit } = todoStore
</script>
  
<style scoped>
  /* Global styles */
  @media (prefers-color-scheme: dark) {
    .todo-app h2 ul li {
      color: #000; /* Adjust text color for dark mode */
    }
  }
  
  /* Section styles */
  section {
    min-height: 100vh;
    padding: 10rem 9% 2rem; /* Adjust padding for different screen sizes */
  }
  
  /* Navbar styles */
  
  .navbar {
    display: flex;
    justify-content: center; /* Center horizontally */
    align-items: center; /* Center vertically */
    width: 100%; /* Make sure the navbar spans the full width */
  }
  
  .navbar a {
    font-size: 1.5rem;
    color: white;
    margin: 0 3rem; /* Adjust margin for spacing between links */
    font-weight: 700;
    text-decoration: none; /* Remove underline from links */
  }  
  
  /* Container styles */
  .container {
    background: -webkit-linear-gradient(90deg, hsla(120, 6%, 90%, 1) 0%, hsla(228, 75%, 16%, 1) 100%);
    filter: progid: DXImageTransform.Microsoft.gradient( startColorstr="#E4E7E4", endColorstr="#0A1647", GradientType=1 );
    padding: 10px;
  }
  
  /* Todo app styles */
  .todo-app {
    width: 100%;
    max-width: 540px;
    background: #fff;
    margin: 20vh auto;
    padding: 40px 30px 70px;
    border-radius: 10px;
  }
  
  /* Todo app header styles */
  .todo-app h2 {
    color: rgb(60, 113, 113); /* Adjust header text color */
  }
  
  /* Row styles */
  .row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: #edeef0;
    border-radius: 30px;
    padding-left: 20px;
  }
  
  /* Input styles */
  input {
    flex: 1;
    border: none;
    outline: none;
    background: transparent;
    padding: 10px;
    font-weight: 14px;
  }
  
  /* Button styles */
  .row button {
    border: none;
    outline: none;
    padding: 16px 50px;
    background: #435a9d;
    color: #fff;
    font-size: 16px;
    cursor: pointer;
    border-radius: 40px;
  }
  
  /* List styles */
  /* List styles */
ul li {
    list-style: none;
    font-size: 17px;
    padding: 12px 8px; /* Adjust padding */
    user-select: none;
    cursor: pointer;
    position: relative;
    
  }
  
  /* List item styles */
  ul li::before {
    content: '';
    position: absolute;
    height: 28px;
    width: 28px;
    border-radius: 50%;
    background-image: url(../assets/unchecked.png);
    background-size: cover;
    background-position: center;
    top: 50%; /* Adjust top position */
    transform: translateY(-50%); /* Center vertically */
    left: 20px;
  }
  
  /* Checked list item styles */
  ul li.checked {
    color: #555;
    text-decoration: line-through;
  }
  
  /* Checked list item icon styles */
  ul li.checked::before {
    background-image: url(../assets/checked.png);
  }
  
  /* Remove button styles */
  .removebutton {
    position: absolute;
    right: 0;
    top: 5px;
    width: 40px;
    height: 40px;
    font-size: 22px;
    color: #555;
    line-height: 40px;
    text-align: center;
    border-radius: 50%;
  }
  
  .removebutton:hover {
    background: #edeef0;
  }
  
  /* Edit button styles */
  .editbutton {
    position: absolute;
    right: 45px; 
    top: 5px;
    width: 40px;
    height: 40px;
    font-size: 16px;
    color: #555;
    line-height: 40px;
    text-align: center;
    border-radius: 50%;
  }
  
  /* Filter styles */
  .filters {
    position: relative;
    padding-top: 3vh;
  }
  
  /* Filter button styles */
  .filters button {
    margin-left: 20px;
  }
  
  /* Button-74 styles */
  .button-74 {

    background-color: #ffffff;
    border: 2px solid #151515;
    border-radius: 30px;
    box-shadow: #333333 4px 4px 0 0;
    color: #000000;
    cursor: pointer;
    display: inline-block;
    font-weight: 600;
    font-size: 18px;
    padding: 0 18px;
    line-height: 50px;
    text-align: center;
    text-decoration: none;
    user-select: none;
    -webkit-user-select: none;
    touch-action: manipulation;
  }
  
  .button-74:hover {
    background-color: #e9e9e9;
  }
  
  .button-74:active {
    box-shadow: #422800 2px 2px 0 0;
    transform: translate(2px, 2px);
  }
  
  /* Media queries */
  @media (min-width: 768px) {
    .button-74 {
      min-width: 120px;
      padding: 0 25px;
    }
  }
  
  @media (max-width: 768px) {
    /* Adjustments for smaller screens */
    .header {
      left: 0;
    }
  }
  </style>
  