<template>
<div>
<nav class="navbar sticky-top navbar-light bg-light">
  <div class="container-fluid">
    <span class="navbar-brand mb-0 h1 text-white">Notes Reminder</span>
  </div>
</nav>
  <div class="container container-fluid mycontainer-background">
    <div v-if="loading" class="text-center">
      <div class="spinner-border text-danger" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-12">
        <div class="mt-4">
          <div >
            <div class="text-right">
            <!-- Trigger the modal with a button -->
            <button
              type="button"
              class="btn btn-default btn-lg mt-2 mybutton float-end"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
            >
              <i class="fas fa-plus-circle"></i> Add a new note
            </button>
</div>
            <!-- Modal -->
            <div
              class="modal fade"
              id="exampleModal"
              tabindex="-1"
              aria-labelledby="exampleModalLabel"
              aria-hidden="true"
            >
              <div class="modal-dialog">
                <!-- Modal content-->
                <div class="modal-content">
                  <div class="modal-header">
                    <h4 class="modal-title">{{modalTitle}}</h4>
                    <button
                      type="button"
                      class="btn-close"
                      data-bs-dismiss="modal"
                      aria-label="Close"
                    ></button>
                  </div>
                  <div class="modal-body">
                    <div class="form-floating mb-3">
                      <input
                        v-model="title"
                        id="title"
                        type="text"
                        class="form-control"
                        placeholder="Enter a title"
                      />
                      <label class="control-label" for="title">Title:</label>
                    </div>
                    <div class="form-floating mb-3">
                      <textarea
                        class="form-control"
                        id="descript"
                        rows="5"
                        v-model="descript"
                      ></textarea>
                      <label class="control-label" for="descript"
                        >Description:</label
                      >
                    </div>
                    <div class="form-floating mb-3">
                      <input
                        type="datetime-local"
                        id="expirationDate"
                        class="form-control"
                        v-model="expirationDate"
                        placeholder="Enter a date"
                      />
                      <label class="control-label" for="expirationDate"
                        >Expiration date:</label
                      >
                    </div>
                    <div class="form-group">
                      <div class="col-sm-offset-2 col-sm-10">
                        <button
                          type="button"
                          v-on:click="addNote()"
                          class="btn btn-default"
                        >
                          Submit
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <div
          v-if="listaNotes.length === 0"
          class="alert alert-success col-md-12"
          role="alert"
        >
          You haven't any note added
        </div>
      </div>
    </div>
    <div class="row mt-2">
      <div v-if="!loading"></div>
      <div v-for="(nota, index) in listaNotes" class="col-md-3" :key="index">
        <div class="card mycards mb-5  cursor-pointer">
          <div class="card-header">
           <small v-bind:class="[
                (nota.expirationDate > nota.creationDate) ? 'text-black' : 'text-danger',
              ]" data-toggle="tooltip" data-placement="top" title="Review this note"> {{ nota.expirationDate }}</small>
          </div>
          <div class="card-body">
            <p><i class="fas fa-star text-warning"></i> {{ nota.title }}</p>
            <hr />
            <p>
              Description: <small>{{ nota.descript }}</small>
            </p>
            <p>
              Creation date: <small>{{ nota.creationDate }}</small>
            </p>
          </div>
          <div class="card-footer justify-content-between">
            <button
              type="button"
              class="btn btn-default btn-sm text-info"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
             v-on:click="getDataUpdate(nota, nota.id)"
            >
                         <!-- v-on:click="updateNote(nota, nota.id)" -->
               <i class="fas fa-pencil-alt"></i> Edit
            </button>
            <button
              type="button"
              class="btn btn-default btn-sm text-danger"
              v-on:click="deleteNote(nota.id)"
            >
              <i class="fas fa-trash-alt"></i> Delete
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
  <footer class="footer">
    <p>&copy; 2021 </p>
  </footer>
</div>

</template>

<script>
import axios from "axios";

const URL="https://backend-tareas20210813141233app.azurewebsites.net/api/Nota/";

export default {
  name: "Notes",
  data() {
    return {
      nota: "",
      listaNotes: [],
      modalTitle:"Add Note",
      loading: false,
      id : 0,
    };
  },
  methods: {
    addNote() {
      const nota = {
        title: this.title,
        descript: this.descript,
        creationDate: new Date(),
        expirationDate: this.expirationDate,
      };
      if(this.id)
      {
        this.updateNote(nota, this.id);
      }
      else{
      
      this.loading = true;
      console.log(nota);
      axios
        .post(URL, nota)
        .then((response) => {
          console.log(response);
          this.loading = false;
          this.getNotes();
          this.removeDataSubmit()
        })
        .catch((error) => {
          this.loading = false;
          console.error(error);
        });
      this.nota = "";
      }
    },
    getDataUpdate(nota, id){
        this.modalTitle="Edit Note";
        this.id= id;
        this.title= nota.title;
        this.descript=nota.descript;
        this.expirationDate=nota.expirationDate;

    },
    updateNote(id, nota) {       
      this.loading= true;
      console.log(id);
      axios.put(URL + id, nota)
        .then((response) => {
          console.log(response);
          alert("Note updated succesfully!");
          this.getNotes();
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
        });
    },
    deleteNote(id) {
      this.loading = true;
      axios
        .delete(URL + id)
        .then((response) => {
          console.log(response);
    alert("Note deleted succesfully!");

          this.getNotes();
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
        });
    },
    getNotes() {
      this.loading=true;
      axios.get(URL).then((response) => {
        console.log(response);
        this.loading=false;
        this.listaNotes = response.data;
      }).catch((error) => {
          console.log(error);
          this.loading = false;
        })
    },
    removeDataSubmit(){
       this.title= "";
        this.descript="";
        this.expirationDate="";
    }
  },
  created: function () {
    this.getNotes();
  },
};
</script>

<style scoped>
.navbar, .navbar-light, .bg-light{
    background-color: rgb(180,1,23) !important;
    color: white !important;
}

.footer {
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
    background-color: rgb(180,1,23);
    color: white;
    text-align: center;
}

.mycontainer-background {
    background: rgb(0,0,0);
    background: linear-gradient(125deg, rgba(0,0,0,1) 3%, rgba(116,116,116,1) 22%, rgba(193,193,193,1) 46%, rgba(196,200,201,1) 100%);
}

.mybutton:hover{
/* font-size: 1.1rem; */
font-weight: bold;
color: white;
}

.mycards{
  height: 20rem !important;
}

.cursor-pointer{
  cursor: pointer;
}
</style>