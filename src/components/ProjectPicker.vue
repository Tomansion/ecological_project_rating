<template>
  <div id="ProjectPicker">
    <!-- Title & projects -->
    <div>
      <h2 id="title">
        Modélisation - Évaluation d'ingénierie écologique - Blue Eco Formations
      </h2>

      <!-- Project list -->
      <span id="projects">
        Projets :
        <button
          :id="'project_' + i"
          :class="'project' + (i == selectedProjectNb ? ' selected' : '')"
          v-for="(project, i) in projects"
          :key="i"
          @click="$emit('selectProject', i)"
          @dblclick="editProjectName(i)"
        >
          {{ project.name }}
        </button>
        <button
          id="addProject"
          v-if="projects.length < maxProjectNumber"
          @click="addProjectBtn"
        >
          Ajouter un projet
        </button>
      </span>
    </div>

    <!-- Logo -->
    <a id="logo" href="https://www.blue-eco-formations.com/">
      <img
        src="https://www.blue-eco-formations.com/wp-content/uploads/2021/07/Logo-Blue-Eco-Formations-1024x306.png"
        alt="Blue Eco Formations"
      />
    </a>

    <!-- new project name modal -->
    <div id="myModal" class="modal" v-show="newProjectModal">
      <form class="modal-content" v-on:submit.prevent>
        <span class="close" @click="newProjectModal = false">&times;</span>
        <h3>Ajout d'un projet</h3>
        <p>
          Nom du nouveau projet :
          <input
            type="text"
            ref="newProjectNameInput"
            v-model="newProjectName"
          />
          <button @click="addProject" type="submit">Ajouter</button>
        </p>
      </form>
    </div>

    <!-- project name edit modal -->
    <div id="myModal" class="modal" v-show="projectNbNameEdit !== null">
      <form class="modal-content" v-on:submit.prevent>
        <span class="close" @click="projectNbNameEdit = null">&times;</span>
        <h3>Modification du nom d'un project</h3>
        <p v-if="projects[projectNbNameEdit]">
          Nom du projet :
          <input
            type="text"
            ref="projectNameInput"
            v-model="projects[projectNbNameEdit].name"
          />
          <button
            @click="
              projectNbNameEdit = null;
              $emit('updateProjectName');
            "
            type="submit"
          >
            OK
          </button>
        </p>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      maxProjectNumber: 5,
      newProjectModal: false,
      projectNbNameEdit: null,
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
        this.$refs.newProjectNameInput.focus();
      });
    },
    addProject() {
      if (this.newProjectName.replace(/\s/g, "").length === 0)
        this.newProjectName = "Projet " + (this.projects.length + 1);
      this.$emit("newProject", this.newProjectName);
      this.newProjectModal = false;
    },
    editProjectName(i) {
      this.projectNbNameEdit = i;
      this.$nextTick(() => {
        this.$refs.projectNameInput.focus();
      });
    },
  },
  watch: {
    newProjectModal() {
      this.newProjectName = "Projet " + (this.projects.length + 1);
    },
  },
};
</script>

<style scoped>
#ProjectPicker {
  padding-left: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
#title {
  display: flex;
  justify-content: space-between;
}
#logo img {
  margin: 10px 10px 0 0;
  width: 450px;
}
@media screen and (orientation: portrait) {
  #ProjectPicker {
    flex-direction: column-reverse;
  }
  #title {
    text-align: center;
  }
  #logo img {
    width: 100%;
    margin: 0 0 10px 0;
  }
}
button {
  cursor: pointer;
  padding: 3px;
  margin-left: 10px;
  border-radius: 3px;
  border: solid rgb(158, 158, 158) 1px;
}

#project_0 {
  background: #1f77b4;
}
#project_1 {
  background: #ff7f0e;
}
#project_2 {
  background: #2ca02c;
}
#project_3 {
  background: #d62728;
}
#project_4 {
  background: #9467bd;
}

.project {
  border: solid cadetblue 1px;
  color: white;
}
.project.selected {
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