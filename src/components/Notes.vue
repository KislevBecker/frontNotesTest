<template>
  <div>
    <h1 class="display-4 text-center">Listado de notas</h1>
    <hr />
    <div class="row">
      <div class="col-lg-8 offset-lg-3">
        <div class="card mt-4">
          <div class="card-body">
            <!-- Trigger the modal with a button -->
            <button
              type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal"
            >
              <i class="fas fa-plus-circle"></i> Add a new note
            </button>

            <!-- Modal -->
            <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
              <div class="modal-dialog">
                <!-- Modal content-->
                <div class="modal-content">
                  <div class="modal-header">
                    <h4 class="modal-title">Create a new note</h4>
                     <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
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
                        v-model="epiration_date"
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
    <div class="row mt-4">
      <div v-if="listaNotes===0" class="alert alert-success col-md-12" role="alert">Hola</div>
            <div v-for="(nota, index) in listaNotes" class="col-md-3" :key="index">
                <div class="card">
                    <div class="card-header">
                        {{nota.expiration_date}}
                    </div>
                    <div class="card-body">
                        <p><span class="glyphicon glyphicon-star"></span> {{nota.title}}</p>
                        <hr />
                        <p>Description: <small>{{nota.descript}}</small></p>
                        <p>Creation date: <small>{{nota.creation_date}}</small></p>
                        <!-- @*<p>{{nota.priority}}</p>*@ -->
                    </div>
                    <div class="card-footer justify-content-between">
                        <button type="button" class="btn btn-default btn-sm" v-on:click="updateNote(nota, index)">
                            <span class="glyphicon glyphicon-pencil"></span> Edit
                        </button>
                        <button type="button" class="btn btn-default btn-sm" v-on:click="deleteNote(index)">
                            <span class="glyphicon glyphicon-trash"></span> Delete
                        </button>
                    </div>
                </div>
            </div>
        </div>
  </div>
</template>

<script>
export default {
  name: "Notes",
  data() {
    return {
      nota: "",
      listaNotes: [],
    };
  },
  methods: {
    addNote() {
      const nota = {
        title: this.title,
        descript: this.descript,
        creation_date: new Date(),
        expiration_date: this.expiration_date,
      };
      this.listaNotes.push(nota);
      this.nota = "";
    },
      updateNote(nota,index){
      this.listaNotes.splice(index, 1)
    },
    deleteNote(index){
      this.listaNotes.splice(index, 1)
    }
  },
};
</script>

<style scoped>
</style>