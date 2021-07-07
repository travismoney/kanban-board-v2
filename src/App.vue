<template>
  <div class="container-lg mt-5">
    <!-- Heading -->
    <h3 class="font-color font-weight-light">Kanban Board V2. 
      <b-button class="btn btn-light btn-circle btn-xl float-right shadow-box btn-add" v-b-modal.modal-prevent-closing><i class="fas fa-plus font-color"></i></b-button>
      <b-modal
        id="modal-prevent-closing"
        ref="modal"
        title="Add New Task"
        @show="resetModal"
        @hidden="resetModal"
        @ok="handleOk"
      >
        <form ref="form" @submit.stop.prevent="handleSubmit">
          <b-form-group
            label="Task"
            label-for="task-input"
            invalid-feedback="Please Add Task!"
            :state="taskState"
          >
            <b-form-input
              id="task-input"
              v-model="newTask"
              :state="taskState"
              required
            >
            </b-form-input>
          </b-form-group>
        </form>
      </b-modal>
   </h3>
    <div class="row mt-5">
    <!-- For Column Backlog -->
      <div class="col-3">
        <div class="p-3 card rounded-corners border-0 shadow-box">
          <h5 class="p-2 font-color font-weight-light">Backlog</h5>
          <!-- Backlog draggable component. Pass arrBackLog to list prop -->
          <draggable
            class="list-group kanban-column"
            :list="arrBacklog"
            group="tasks"
          >
            <div
              class="list-group-item font-color font-weight-light mb-3 shadow-box rounded-corners border-0"
              v-for="(element,index) in arrBacklog"
              :key="element.name"
            >
              <div class="font-small">
                {{ element.name }}
                <i class="far fa-times-circle float-right delete-circle" @click="removeFromBacklog(index)"></i>
              </div>
            </div>
          </draggable>
        </div>
      </div>

      <!-- For Column In Progress -->
      <div class="col-3">
        <div class="p-3 card rounded-corners border-0 shadow-box">
          <h5 class="p-2 font-color font-weight-light">In Progress</h5>
          <!-- Backlog draggable component. Pass arrBackLog to list prop -->
          <draggable
            class="list-group kanban-column"
            :list="arrInProgress"
            group="tasks"
          >
            <div
              class="list-group-item font-color font-weight-light mb-3 shadow-box rounded-corners border-0"
              v-for="(element,index) in arrInProgress"
              :key="element.name"
            >
              <div class="font-small">
                {{ element.name }}
                <i class="far fa-times-circle float-right delete-circle" @click="removeFromInProgress(index)"></i>
              </div>
            </div>
          </draggable>
        </div>
      </div>

      <!-- For Column In Review  -->
      <div class="col-3">
        <div class="p-3 card rounded-corners border-0 shadow-box">
          <h5 class="p-2 font-color font-weight-light">In Review</h5>
          <!-- Backlog draggable component. Pass arrBackLog to list prop -->
          <draggable
            class="list-group kanban-column"
            :list="arrInReview"
            group="tasks"
          >
            <div
              class="list-group-item font-color font-weight-light mb-3 shadow-box rounded-corners border-0"
              v-for="(element,index) in arrInReview"
              :key="element.name"
            >
              <div class="font-small">
                {{ element.name }}
                <i class="far fa-times-circle float-right delete-circle" @click="removeFromInReview(index)"></i>
              </div>
            </div>
          </draggable>
        </div>
      </div>

      <!-- For Column Completed -->
      <div class="col-3">
        <div class="p-3 card rounded-corners border-0 shadow-box">
          <h5 class="p-2 font-color font-weight-light">Completed</h5>
          <!-- Backlog draggable component. Pass arrBackLog to list prop -->
          <draggable
            class="list-group kanban-column"
            :list="arrCompleted"
            group="tasks"
          >
            <div
              class="list-group-item font-color font-weight-light mb-3 shadow-box rounded-corners border-0"
              v-for="(element,index) in arrCompleted"
              :key="element.name"
            >
              <div class="font-small">
                {{ element.name }}
                <i class="far fa-times-circle float-right delete-circle" @click="removeFromCompleted(index)"></i>
              </div>
            </div>
          </draggable>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

// import draggable
import draggable from "vuedraggable";

export default {

  name: "kanban-board",

  components: {
    // import draggable as a component
    draggable
  },

  data() {
    return {
      // variables for new tasks
      newTask: "",
      taskState: null,

      // Initialized first arrray with some data
      arrBacklog: [
        { name: "Debug Scroll Issue" },
        { name: "Design Wireframe" },
        { name: "Prepare SRS" },
        { name: "Prepare SDS" },
        { name: "Test System" }
      ],

      // Empty arrays
      arrInProgress: [],
      arrInReview: [],
      arrCompleted: []

    };

  },
  
  methods: {

    // check form is filled 
    checkFormValidity() {
        const valid = this.$refs.form.checkValidity()
        this.taskState = valid
        return valid
      },

    // reset input fields
    resetModal() {
      this.newTask = "";
      this.taskState = "";
    },

    handleOk(bvModalEvt) {
      // Prevent modal from closing
      bvModalEvt.preventDefault()
      // Trigger submit handler
      this.handleSubmit()
    },

    handleSubmit() {
      // Exit when the form isn't valid
      if (!this.checkFormValidity()) {
        return
      }
      // Push the push new tasks to arrBacklog
        this.arrBacklog.push({ name: this.newTask });
        this.newTask = "";
        // Hide the modal manually
        this.$nextTick(() => {
          this.$bvModal.hide('modal-prevent-closing')
        })
      },

    // delete task from backlog array
    removeFromBacklog(id){
      this.$delete(this.arrBacklog, id);
    },

    // delete task from in progress array
    removeFromInProgress(id){
      this.$delete(this.arrInProgress, id);
    },

    // delete task from in review array
    removeFromInReview(id){
      this.$delete(this.arrInReview, id);
    },

    // delete task from completed array
    removeFromCompleted(id){
      this.$delete(this.arrCompleted, id);
    }
  }
};
</script>

<style>
/* import styles.css */
@import './assets/styles/styles.css';
</style>
