<template>
  <div class="archive main-title">
    <h2>Archived Notes</h2>
    <div v-if="showFullNote" @click="showFullNote = false" id="cover"/>
    <div class="notes">
      <FullNote @close="filterNotes" :data="currentNoteData" v-if="showFullNote" />
      <div v-if="showFullNote" @click="showFullNote = false" id="cover" />
      <div class="noteCover" v-for="note in notes" :key="note.id">
        <div
          v-if="note.archived"
          @click="currentNoteDataUpdater(note)"
          class="note"
          :style="'background:'+note.color+';'"
        >
          <h2>{{note.title}}</h2>
          <h3>{{note.content}}</h3>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import firebase from "firebase";
import db from "@/firebase/init";
import FullNote from '@/components/FullNote'
export default {
  name: "archive",
  data() {
    return {
      notes: [],
      showFullNote: false,
      currentNoteData:null
    };
  },
  components:{
      FullNote
  },
  created() {
    //logins user
    firebase.auth().onAuthStateChanged(() => {
      //route guard
      let user = firebase.auth().currentUser;
      if (!user) {
        this.$router.push({ name: "Login" });
      } else {
        //gets notes/updates
        let ref = db
          .collection("notes")
          .where("owner", "==", firebase.auth().currentUser.uid)
          .where("archived", "==", true);
        ref.onSnapshot((snap) => {
          snap.docChanges().forEach((change) => {
            let note = change.doc.data();
            if (change.type == "added") {
                let newNote = note;
                newNote.id = change.doc.id;
                newNote.big = false;
                newNote.index = this.notes.length;
                newNote.hover = false;
                this.notes.push(newNote);
            }
            else if(change.type == 'modified'){
              let unEditedNote = this.notes.filter(item => {
                return item.id == change.doc.id
              })
              let updatedNote = note
              updatedNote.id = unEditedNote[0].id
              updatedNote.index = unEditedNote[0].index
              this.notes[unEditedNote[0].index] = updatedNote
            }else if (change.type == "removed") {
              this.notes = this.notes.filter((item) => {
                return item.id != change.doc.id;
              });
            }
          });
        });
      }
    });
  },
  methods:{
    currentNoteDataUpdater(note){
      this.currentNoteData = {
        title:note.title,
        content:note.content,
        color:note.color,
        id:note.id,
        archived:note.archived
      }
      this.showFullNote = true
    },
    filterNotes(){
        // this.notes = this.notes.filter(note => {
        //     return note.id != test
        // })
        this.showFullNote = false
    }
  }
};
</script>
<style scoped>
.main-title {
  text-align: center;
}
#cover{
  opacity: 0.8;
  position: fixed;
  background: gray;
  width: 100%;
  height: 100%;
  z-index: 1;
  left: 0px;
  top: 0;
}
</style>