<template>
<main @keyup.esc="clearClose">
  <div v-if="showModal" class="overlay">
    <div class="modal">
      <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
      <p v-if="errorMessage">{{errorMessage}}</p>
      <p v-if="sucESS">{{success}}</p>
      <button @click="addNote">Add Note</button>
      <button @click="clearClose"  class="close">Close</button>
    </div>
  </div>
  <div class="container">
    <header>
      <h1>Notes </h1>
      <div class="btn">
      <button @click="showModal = true">+</button>
      <button @click="deleteNote" class="deleteNote">-</button>
      </div>
    </header>
    <div class="cards-container">
      <p class="default" v-if="emptyNote">The list of notes is empty</p>
      <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{backgroundColor: note.backgroundColor}"
      >
        <p class="main-text">{{note.text}}</p>
        <p class="date">{{note.date.toLocaleDateString("ru-US")}}</p>
      </div>
    </div>
  </div>
</main>
</template>

<script setup>
import {computed, ref} from "vue";

 const showModal = ref(false)
 const newNote = ref("")
 const errorMessage = ref("")
 const notes = ref([]);
 const emptyNote = ref(true)
 const success = ref('')

 const minLength = 10;
 const formIncorrect = computed(()=> newNote.value.trim().length < minLength)


function sucESS() {
   if (newNote.value.trim().length > minLength) {
     return success.value = "Success input";
   }
}

 function getRandomColor() {
   return "hsl(" + Math.random()*360+ ", 100%, 75%)";
 }

 function deleteNote() {
   if (notes.value.length > 0) {
     notes.value.pop();
   } else {
     emptyNote.value = true
   }
 }

 function clearClose() {
   showModal.value = false;
   newNote.value = '';
   errorMessage.value = "";
 }

 function addNote(){
   if (formIncorrect.value) {
     return errorMessage.value = "Note needs to be 10 characters or more";
   }
   notes.value.push({
     id: Math.floor(Math.random()*1000000),
     text: newNote.value,
     date: new Date(),
     backgroundColor: getRandomColor()
   });
   emptyNote.value = false;
   showModal.value = false;
   newNote.value = "";
   errorMessage.value = "";
 }

</script>

<style lang="scss" scoped>
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
  color: black;
}
header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21,20,20);
  border-radius: 100%;
  color: #ffffff;
  font-size: 20px;
}
.btn .deleteNote {
  margin-left:10px;
}
.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237,182,44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  overflow: hidden;
}
.card .main-text {
  overflow: hidden;
}
.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}
.cards-container .default {
  font-size: 30px;
  color: gray;
  text-transform:  uppercase;
}
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0,0,0,0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal textarea {
  border-radius: 5px;
  border: 1px solid black;
  outline: black;
}

.modal textarea:focus {
  border-radius: 5px;
  border: 3px solid blueviolet;
  outline: blueviolet;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}
.modal .close {
  background-color: rgb(193,15,15);
  margin-top: 7px;
}

.modal p {
  color: red;
}
</style>
