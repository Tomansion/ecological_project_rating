<template>
  <div id="ProjectPicker">
    <h2>
      Modélisation - Évaluation d'ingénierie écologique - Blue Eco Formations
    </h2>
    <span>
      Projects :
      <button
        :class="'project' + (i == selectedProjectNb ? ' selected' : '')"
        v-for="(project, i) in projects"
        :key="i"
        @click="$emit('selectProject', i)"
      >
        {{ project.name }}
      </button>
      <button
        id="addProject"
        v-if="projects.length < maxProjectNumber"
        @click="addProjectBtn"
      >
        + Add project
      </button>
    </span>

    <!-- project name modal -->
    <div id="myModal" class="modal" v-show="newProjectModal">
      <form class="modal-content" v-on:submit.prevent>
        <span class="close" @click="newProjectModal = false">&times;</span>
        <h3>Adding a project</h3>
        <p>
          New project name :
          <input type="text" ref="projectNameInput" v-model="newProjectName" />
          <button @click="addProject" type="submit">Add</button>
        </p>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      maxProjectNumber: 12,
      newProjectModal: false,
      newProjectName: "",
    };
  },
  props: {
    projects: { type: Array, required: true },
    selectedProjectNb: { type: Number, required: true },
  },
  methods: {
    addProjectBtn() {
      this.newProjectModal = true;
      this.$nextTick(() => {
        this.$refs.projectNameInput.focus();
      });
    },
    addProject() {
      if (this.newProjectName.replace(/\s/g, "").length === 0)
        this.newProjectName = "Project " + (this.projects.length + 1);
      this.$emit("newProject", this.newProjectName);
      this.newProjectModal = false;
    },
  },
  watch: {
    newProjectModal() {
      this.newProjectName = "Project " + (this.projects.length + 1);
    },
  },
};
</script>

<style scoped>
#ProjectPicker {
  padding: 10px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
button {
  padding: 3px;
  margin-left: 10px;
  border-radius: 3px;
  border: solid rgb(158, 158, 158) 1px;
}
.project {
  border: solid cadetblue 1px;
  color: rgb(25, 78, 80);
}
.project.selected {
  color: green;
  font-weight: bold;
  border: solid green 1px;
}
#addProject {
  color: rgb(66, 66, 66);
  font-size: 0.7em;
}

/* Modal */

/* The Modal (background) */
.modal {
  position: fixed; /* Stay in place */
  z-index: 10; /* Sit on top */
  padding-top: 100px; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0, 0, 0); /* Fallback color */
  background-color: rgba(0, 0, 0, 0.4); /* Black w/ opacity */
}

/* Modal Content */
.modal-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 1px solid #888;
  width: 50%;
}

/* The Close Button */
.close {
  color: #aaaaaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}
</style>