<template>
  <div class="container container-fluid">
    <h1 class="display-4 text-center">Listado de notas</h1>
    <hr />

    <div v-if="loading" class="text-center">
      <div class="spinner-border text-danger" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-8 offset-lg-3">
        <div class="card mt-4">
          <div class="card-body">
            <!-- Trigger the modal with a button -->
            <button
              type="button"
              class="btn btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
            >
              <i class="fas fa-plus-circle"></i> Add a new note
            </button>

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
                    <h4 class="modal-title">Create a new note</h4>
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
                        id="expiration_date"
                        class="form-control"
                        v-model="expiration_date"
                        placeholder="Enter a date"
                      />
                      <label class="control-label" for="expiration_date"
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
    <div class="row mt-4">
      <div v-if="!loading"></div>
      <div v-for="(nota, index) in listaNotes" class="col-md-3" :key="index">
        <div class="card">
          <div class="card-header">
            {{ nota.expiration_date }}
          </div>
          <div class="card-body">
            <p><i class="fas fa-star"></i> {{ nota.title }}</p>
            <hr />
            <p>
              Description: <small>{{ nota.descript }}</small>
            </p>
            <p>
              Creation date: <small>{{ nota.creation_date }}</small>
            </p>
            <!-- @*<p>{{nota.priority}}</p>*@ -->
          </div>
          <div class="card-footer justify-content-between">
            <button
              type="button"
              class="btn btn-default btn-sm"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
             v-on:click="updateNote(nota.id)"
            >
               <i class="fas fa-pencil-alt"></i> Edit
            </button>
            <button
              type="button"
              class="btn btn-default btn-sm"
              v-on:click="deleteNote(nota.id)"
            >
              <i class="fas fa-trash-alt"></i> Delete
            </button>
          </div>
        </div>
      </div>
    </div>
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
      modalTitle:"",
      loading: false,
    };
  },
  methods: {
    addNote() {
      this.modalTitle="Add Note";
      const nota = {
        title: this.title,
        descript: this.descript,
        creation_date: new Date(),
        expiration_date: this.expiration_date,
      };
      this.loading = true;
      console.log(nota);
      axios
        .post(URL, nota)
        .then((response) => {
          console.log(response);
          this.loading = false;
          this.getNotes();
        })
        .catch((error) => {
          this.loading = false;
          console.error(error);
        });
      this.nota = "";
    },
    updateNote(nota, id) {
      this.modalTitle="Edit Note";
      this.loading= true;
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
    }
  },
  created: function () {
    this.getNotes();
  },
};
</script>

<style scoped>
</style>