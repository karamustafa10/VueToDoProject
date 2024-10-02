<template>

  <div style="display: flex; justify-content: center; align-items: center;">
    <div style="text-align: center;">
      <input type="text" placeholder="Yapılacak İşinizi Buraya Yazın..." v-model="inputText" />
      <br />
      <button class="sendButton" style="margin-top: 5%;" @click="addToList">Yapılacaklar Listesine Ekle</button>
    </div>

    <div class="card">
      <h1>{{ kartBasligi }}</h1>
      <hr />
      <div>
        <ul style="max-height: 45vh; overflow-y: auto;">
          <li v-for="(task, index) in tasks" :key="index">
            {{ index + 1 }}- {{ task.gorev }}
            <button :id="index" class="doneButton" @click="clickDoneButton(index)"><img src="./done.png" alt="done" /></button>
            <button :id="index" class="deleteButton" @click="clickDeleteButton(index)"><img src="./delete.png" alt="delete" /></button>
          </li>
        </ul>
      </div>
    </div>
  </div>

</template>

<script>

import { ref, onMounted } from 'vue';

export default {
  setup() {
    const kartBasligi = "Yapılacaklar";
    const inputText = ref("");
    const tasks = ref([]);


    const loadTasks = () => {
      const storedTasks = localStorage.getItem('tasks');
      if (storedTasks) {
        tasks.value = JSON.parse(storedTasks);
      }
    };


    const saveTasks = () => {
      localStorage.setItem('tasks', JSON.stringify(tasks.value));
    };

    const addToList = () => {
      if (inputText.value === "") {
        alert("Lütfen yapılacak bir iş giriniz...");
      } else {
        tasks.value.push({ gorev: inputText.value, durum: false });
        inputText.value = "";
        saveTasks();
      }
    };

    const clickDoneButton = (index) => {
      tasks.value.splice(index, 1);
      saveTasks();
    };

    const clickDeleteButton = (index) => {
      tasks.value.splice(index, 1);
      saveTasks();
    };

    onMounted(loadTasks);

    return { kartBasligi, inputText, tasks, addToList, clickDoneButton, clickDeleteButton };
  },
};

</script>

<style>
  body {
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #010D1A;
  }

  h1 {
    color: white;
  }

  li {
    display: flex;
    align-items: center;
    padding: 2%;
    padding-left: 0;
    color: white;
  }

  input {
    padding-left: 2%;
    padding-right: 2%;
    padding-top: 1%;
    padding-bottom: 1%;
    color: white;
    background-color: #3B3F43;
    width: 25vw;
    height: 35vh;
    border-radius: 10px;
    border: 3px solid white;
  }

  img {
    padding: 0;
    margin: 0;
    width: 25px;
    height: 25px;
    border: none;
  }

  .card {
    width: 50vw;
    height: 60vh;
    padding: 2%;
    margin-left: 20%;
    background-color: #3B3F43;
    border-radius: 20px;
    border: 3px solid white;
  }

  .sendButton {
    padding: 5px;
    background-color: #3B3F43;
    color: white;
    width: 50%;
    height: 20%;
    border: 3px solid white;
  }

  .doneButton,
  .deleteButton {
    padding: 0;
    margin-left: 2%;
    background: none;
    border: none;
  }
</style>

