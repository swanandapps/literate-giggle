/* eslint-disable vue/no-dupe-keys */
/* eslint-disable vue/no-dupe-keys */
<template>
  <div class="hello">
    <div>
      <div class="item">
        <h1>Hello, Swanand Kadam</h1>

        <img src="../assets/0.jpg" class="profile" alt="" />
      </div>

      <p>You have {{ TaskList.length }} tasks</p>
    </div>
    <div class="grid">
      <div :key="index" v-for="(item, index) in TaskList">
        <drag
          class="drag"
          @dragstart="drag()"
          @drop="handleDrop(item, index)"
          :key="item"
          :class="{ [item]: true }"
          :transfer-data="{
            item: item,
            TaskList: TaskList,
            example: 'lists',
            index: index,
          }"
        >
          <md-toolbar
            v-if="item.completed == false"
            color="primary"
            class="toolbar"
          >
            <div class="item">
              <h3>{{ item.title }}</h3>

              <md-icon>alarm</md-icon>
            </div>

            <p>Due {{ item.due }}</p>
          </md-toolbar>
        </drag>

        <md-toolbar
          v-if="item.completed == true"
          color="primary"
          class="toolbar done-toolbar"
        >
          <drag class="drag" :transfer-data="{ draggable }">
            <div class="Done-item">
              <div class="icon">
                <md-icon>done</md-icon>
              </div>
              <div class="metadata">
                <h3>{{ item.title }}</h3>

                <p>{{ item.due }}</p>
              </div>
            </div>
          </drag>
        </md-toolbar>
      </div>
    </div>

    <div style="margin-top: 5%">
      <drop v-if="this.dragged == false" class="drop" @drop="handleDrop">
        <md-button @click="openAddTask()" class="md-fab md-primary">
          <md-icon>add</md-icon>
        </md-button>
      </drop>

      <drop v-if="this.dragged == true" class="drop" @drop="handleDrop">
        <md-button class="md-fab">
          <md-icon>delete</md-icon>
        </md-button>
      </drop>
    </div>

    <div v-if="ShowAddTask" class="addtask__container">
      <div class="closeBtn">
        <svg
          @click="Close()"
          aria-hidden="true"
          focusable="false"
          data-prefix="fas"
          data-icon="times"
          class="svg-inline--fa fa-times fa-w-11"
          role="img"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 352 512"
        >
          <path
            fill="currentColor"
            d="M242.72 256l100.07-100.07c12.28-12.28 12.28-32.19 0-44.48l-22.24-22.24c-12.28-12.28-32.19-12.28-44.48 0L176 189.28 75.93 89.21c-12.28-12.28-32.19-12.28-44.48 0L9.21 111.45c-12.28 12.28-12.28 32.19 0 44.48L109.28 256 9.21 356.07c-12.28 12.28-12.28 32.19 0 44.48l22.24 22.24c12.28 12.28 32.2 12.28 44.48 0L176 322.72l100.07 100.07c12.28 12.28 32.2 12.28 44.48 0l22.24-22.24c12.28-12.28 12.28-32.19 0-44.48L242.72 256z"
          ></path>
        </svg>
      </div>
      <input
        type="text"
        name=""
        placeholder="What would you like to add ?"
        v-model="Task"
        class="md-inline"
        autofocus
      />
    </div>
    <md-button
      @click="addTask()"
      v-if="ShowAddTask"
      class="md-raised md-primary AddButton"
    >
      <md-icon>add</md-icon>
    </md-button>
  </div>
</template>

<script>
import { Drag, Drop } from "vue-drag-drop";

export default {
  name: "Home",
  components: { Drag, Drop },
  data() {
    return {
      TaskList: [
        {
          title: "Complete Ten Push Ups",
          due: "Sun, Dec 20",
          completed: false,
        },

        {
          title: "Interview scheduled for Mike",
          due: "Wed, Dec 25",
          completed: false,
        },
        {
          title: "Meeting",
          due: "Sat, Dec 30",
          completed: false,
        },
        {
          title: "Party at Jason's House",
          due: "Thu, Dec 10",
          completed: true,
        },
      ],
      Task: "",
      CurrenTasK: "",
      ShowAddTask: false,
      dragged: false,
    };
  },
  methods: {
    openAddTask() {
      this.ShowAddTask = true;
    },
    handleDrop(item) {
      let index = item.index;
      this.TaskList.splice(index, 1);
      this.dragged = false;
    },
    drag() {
      this.dragged = true;
      console.log(this.dragged);
    },

    addTask() {
      var date = new Date();
      date = date.toString();
      date = date.substring(0, 10);

      var TaskObject = { title: "", due: date, completed: false };

      TaskObject.title = this.Task;
      TaskObject.due = date;

      this.TaskList.push(TaskObject);
      this.ShowAddTask = false;
    },
    Close() {
      this.ShowAddTask = false;
    },

    removeTask(index) {
      this.TaskList.splice(index, 1);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  display: grid;
  width: 50%;
  margin: auto;
}
.item {
  display: grid;
  grid-template-columns: 14fr 1fr;
  width: 52em;
}
.grid {
  display: grid;
  grid-template-columns: 1fr;

  justify-content: center;
  margin-top: 5%;
}
.done-toolbar {
  padding: 0%;
}
img {
  margin-left: 64%;
}
.Done-item {
  display: grid;
  grid-template-columns: 5fr 14fr;
  height: 71px;
}

.icon {
  text-align: center;
  margin: auto;
  width: 100%;
  background-color: greenyellow;
  height: 71px;
}
.md-icon {
  width: 100%;
  height: 100%;
}

.metadata {
  display: block;
  margin-left: 5%;
  width: 100%;
  text-align: left;
}

.metadata h3 {
  margin-bottom: 2%;
}
.item :nth-child(1) {
  text-align: left;
}

.item :nth-child(2) {
  margin-right: 2%;
}

p {
  text-align: left;
  margin-bottom: 1%;
  margin-top: 0%;
}

.toolbar {
  width: 100%;
  background: white;

  min-height: 70px;
  cursor: move;
  margin: auto;
  margin-bottom: 20px;
  box-shadow: 0px 3px 8px -1px rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  display: grid;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.md-title {
  color: black;
  font-weight: 900;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.addtask__container {
  position: absolute;
  left: 0;
  right: 0;
  margin-left: auto;
  margin-right: auto;
  width: 100px;
  z-index: 2000;
  background: whitesmoke;
  height: 82%;
  width: 55%;
  box-shadow: 0px 3px 8px -1px rgba(0, 0, 0, 0.2);
}

input[type="text"] {
  width: 100%;
  margin: 0 5%;
  height: 70px;
  /* box-shadow: var(--shadow); */
  border: none;
  border-radius: 8px;
  padding: 10px 10px;
  font-size: 25px;
  font-weight: 500;
  outline: none;
  background: transparent;
  text-align: left;
}
.AddButton {
  position: absolute;
  left: 0;
  right: 0;
  margin-left: auto;
  margin-right: auto;
  width: 100px;
  z-index: 2000;
  background: whitesmoke;
  height: 10%;
  width: 55%;
  box-shadow: 0px 3px 8px -1px rgba(0, 0, 0, 0.2);
  top: 80%;
}

.svg-inline--fa {
  display: inline-block;
  font-size: inherit;
  height: 1em;
  overflow: visible;
  vertical-align: -0.125em;
}

input[type="text"]::placeholder {
  color: #ccc;
}

.closeBtn {
  cursor: pointer;
  font-size: 35px;
  padding-left: 10px;
  padding-top: 15px;
  margin-left: 5%;
  text-align: left;
  width: 100%;
}

.profile {
  width: 60px;
  border-radius: 50%;
}
</style>
