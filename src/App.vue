<script setup>

import { ref } from 'vue';

const addNoteState = ref(false)
const modifyNoteState = ref(false)
const textAreaContent = ref("")
const mainId = ref(2)
const noteErrorDisplayState = ref(false) 
const currentModifiedNoteIndex = ref(null)
const notes = ref([
  {
  content: 'boilerplate note',
  date: '11/26/2023',
  backgroundC: `hsl(${Math.random() * 360}, 100%, 75%)`,
  id: 0
},
{
  content: 'vue is goated',
  date: '11/27/2023',
  backgroundC: `hsl(${Math.random() * 360}, 100%, 75%)`,
  id: 1
}
])

function noteValidation() {
  noteErrorDisplayState.value = true;
   return (
    textAreaContent.value.length <= 10) ? false : true
}

function addNote() {
  console.log(noteValidation())
  if (noteValidation()) {
    notes.value.push({
      content: textAreaContent.value,
      date: new Date().toLocaleDateString('en-US'),
      backgroundC: `hsl(${Math.random() * 360}, 100%, 75%)`,
      id: mainId.value
    })
    mainId.value++;
    textAreaContent.value = "";
    addNoteState.value = false;
    noteErrorDisplayState.value = false;
  }
}

const openModifyNoteModule = (currentNote) => {
  currentModifiedNoteIndex.value = notes.value.indexOf(currentNote)
  modifyNoteState.value = true;
}

const modifyNote = () => {

if (noteValidation()) {
  notes.value[currentModifiedNoteIndex.value].content = textAreaContent.value;
  textAreaContent.value = '';
  modifyNoteState.value = false;
  noteErrorDisplayState.value = false;
}

}

function deleteNote() {
  notes.value = notes.value.filter((a) => {
   return a != notes.value[currentModifiedNoteIndex.value]
  })
  modifyNoteState.value = false;
}

</script>

<template>
  <div v-if="addNoteState || modifyNoteState" class="addNoteOverlay">
    <div class="noteModuleContainer" v-if="addNoteState">
      <div class="moduleHeader">
        <h3 class="subHeader">Add a note</h3>
        <button class="closeAddNoteOverlay" @click="addNoteState = false">x</button>
      </div>
      <h5 class="noteErrorMessage" v-show="noteErrorDisplayState">Your note must consist of at least 10 characters!</h5>
      <textarea name="noteContent" id="noteContent" cols="50" rows="20" v-model.trim="textAreaContent"></textarea>
      <button class="addNoteOverlayButton" @click="addNote">Add note</button>
    </div>

    <div class="noteModuleContainer" v-if="modifyNoteState">
      <div class="moduleHeader">
        <h3 class="subHeader">Modify a note</h3>
        <button class="closeAddNoteOverlay" @click="modifyNoteState = false">x</button>
      </div>
      <h5 class="noteErrorMessage" v-show="noteErrorDisplayState">Your note must consist of at least 10 characters!</h5>
      <textarea name="noteContent" id="noteContent" cols="50" rows="20" v-model.trim="textAreaContent"></textarea>
      <button class="removeNoteOverlayButton" @click="deleteNote">Delete note</button>
      <button class="addNoteOverlayButton" @click="modifyNote">Modify note</button>
    </div>

  </div>
  <main>
    <header>
        <h1>Notes</h1>
        <button class="addNoteButton" @click="addNoteState = true">+</button>
    </header>

    <div class="notesContainer">
      <div class="noteCard" v-for="note in notes" :key="note.id" :style="{backgroundColor: note.backgroundC}" @click="openModifyNoteModule(note)">
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
    padding: 30px;
    display: flex;
    justify-content: center;
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
    overflow-wrap: break-word;
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
    z-index: 2;
  }

  .noteModuleContainer {
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

  .addNoteOverlayButton, .removeNoteOverlayButton {
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

  .removeNoteOverlayButton {
    background-color: white;
    color: #960018;
    border: 1px solid #960018;
  }

  .removeNoteOverlayButton:hover {
    background-color: #960018;
    color: white;
  }

  .moduleHeader {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
  }

  .subHeader {
    font-size: 33px;
    font-weight: 500;
  }

  .noteErrorMessage {
    color: red;
    font-size: 20px;
    font-weight: 500;
    align-self: flex-start;
  }

</style>