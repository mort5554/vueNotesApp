<script setup>

import { ref } from 'vue';

const overlayState = ref(false)
const textAreaContent = ref(null)
const mainId = ref(0)
const notes = ref([{
  content: 'new me same me',
  date: '1-12-3-3123',
  backgroundC: `hsl(${Math.random() * 360}, 100%, 75%)`
}])

function addNote() {
  notes.value.push({
    content: textAreaContent.value,
    date: new Date().toLocaleDateString('en-US'),
    backgroundC: `hsl(${Math.random() * 360}, 100%, 75%)`,
    id: mainId
  })
  mainId.value++;
  textAreaContent.value = null;
  overlayState.value = false;
}

</script>

<template>
  <div v-if="overlayState" class="addNoteOverlay">
    <div class="addNoteContainer">
      <button class="closeAddNoteOverlay" @click="overlayState = false">x</button>
      <textarea name="noteContent" id="noteContent" cols="50" rows="20" v-model="textAreaContent"></textarea>
      <button class="addNoteOverlayButton" @click="addNote">Add note</button>
    </div>
  </div>
  <main>
    <header>
        <h1>Notes</h1>
        <button class="addNoteButton" @click="overlayState = true">+</button>
    </header>

    <div class="notesContainer">
      <div class="noteCard" v-for="note in notes" :key="note.id" :style="{backgroundColor: note.backgroundC}">
        <p>{{ note.content }}</p>
        <p>{{ note.date }}</p>
      </div>
    </div>
  </main>
</template>

<style scoped>

  * {
    font-family: 'Quicksand', sans-serif;
  }

  main {
    width: 1200px;
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 0 auto;
  }

  header {
    width: 60%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 35px;
    font-weight: 700;
    padding: 20px;
  }


  .addNoteButton {
    background-color: black;
    color: white;
    border-radius: 100%;
    border: none;
    width: 50px;
    height: 50px;
    margin: 0px;
    padding: 10px;
    font-size: 15px;
    cursor: pointer;
    transition: .1s all;
    border: 1px solid white;
  }

  .addNoteButton:hover {
    background-color: white;
    color: black;
    border: 1px solid black;
  }

  .notesContainer {
    align-self: flex-start;
    padding: 30px;
    display: flex;
    gap: 25px;
    flex-wrap: wrap;
  }

  .noteCard {
    width: 250px;
    min-height: 250px;
    padding: 15px;
    border: 1px solid black;
    border-radius: 15px;
    font-size: 18px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }

  .addNoteOverlay {
    position: absolute;
    width: 100vw;
    height: 100vh;
    top: 0;
    left: 0;
    background-color: rgba(0, 0, 0, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .addNoteContainer {
    padding: 35px;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: white;
    border: 1px solid black;
    border-radius: 15px;
    gap: 25px;
  }

  .closeAddNoteOverlay {
    align-self: flex-end;
    border: 1px solid black;
    background-color: white;
    color: black;
    width: 40px;
    height: 40px;
    border-radius: 100%;
    cursor: pointer;
    font-size: 16px;
    text-align: center;
    transition: .1s all;
  }

  .closeAddNoteOverlay:hover {
    background-color: brown;
    color: white;
  }

  textarea {
    outline: black;
    resize: none;
    font-size: 16px;
  }

  .addNoteOverlayButton {
    width: 100%;
    background-color: white;
    color: black;
    border-radius: 15px;
    border: 1px solid black;
    padding: 15px;
    font-size: 18px;
    transition: .1s all;
    cursor: pointer;
  }

  .addNoteOverlayButton:hover {
    background-color: black;
    color: white;
  }

</style>