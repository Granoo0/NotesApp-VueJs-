<script setup>
import { ref } from "vue";

const toggleModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const errorMessage = ref(false);

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const pushNote = () => {
  if (newNote.value.length < 9) {
    errorMessage.value = true;
    return;
  } else 
  notes.value.push({
    id: Math.floor(Math.random() * 99999999),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });
  toggleModal.value = false;
  newNote.value = "";
  errorMessage.value = false;
};
</script>

<template>
  <main>
    <div class="overlay" v-if="toggleModal">
      <div class="modal">
        <button class="close_btn" @click="toggleModal = false">X</button>
        <textarea
          name="notes"
          id="notes"
          cols="30"
          rows="10"
          v-model.trim="newNote"
        ></textarea>
        <p class="error_msg" v-if="errorMessage">The note must be at least 10 characters long!</p>
        <button class="modal_btn" @click="pushNote">Add note</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button class="header_btn" @click="toggleModal = true">+</button>
      </header>
      <div class="cards-container">
        <div
          class="card"
          v-for="note in notes"
          :key="note.id"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-EU") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

.header_btn {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(209, 153, 57);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  color: rgb(49, 49, 49);
  margin-right: 20px;
  margin-bottom: 20px;
  overflow: hidden;
}

.main-text {
  font-size: 20px;
  font-weight: 400;
}

.date {
  font-size: 14px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 99;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: #fff;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}
.modal_btn {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: #fff;
  cursor: pointer;
  margin-top: 15px;
}

.close_btn {
  background-color: rgb(230, 41, 41, 0.77);
  width: 28px;
  height: 28px;
  border-radius: 50%;
  border: none;
  color: #fff;
  margin-bottom: 10px;
  left: 95%;
  bottom: 15px;
  font-size: 15px;
  cursor: pointer;
}

textarea {
  font-size: 25px;
  resize: none;
  max-height: 200px;
}

.error_msg {
  color: rgb(234, 12, 12);
  font-size: 16px;
  font-weight: 500;
}
</style>
