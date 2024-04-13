<template>
  <h2>ToDo List</h2>
  <!-- <RegularButtonComponent :buttonName="'Delete Done ToDos'" /> -->
  <ul>
    <template v-for="(toDo, index) of toDoList" :key="toDo.id">
      <li v-if="shouldToDoBeRendered(toDo)">
        <label
          :for="toDo.id"
          :class="{
            doneToDo: toDo.done,
          }"
          >{{ toDo.description }}</label
        ><input
          type="checkbox"
          :id="toDo.id"
          :checked="toDo.done"
          @click.prevent="changeToDoState(toDo, index)"
        />
      </li>
    </template>
  </ul>
</template>

<script>
// import RegularButtonComponent from "@/components/RegularButtonComponent.vue";

export default {
  data() {
    return {
      urlOfBackend: "http://localhost:4730/todos/",
      toDoList: [],
    };
  },
  name: "ToDoList",
  components: {
    // RegularButtonComponent,
  },
  props: {
    whichRadioState: String,
  },
  methods: {
    shouldToDoBeRendered(currentToDoOfToDoList) {
      if (
        this.whichRadioState === "allButton" ||
        (this.whichRadioState === "openButton" &&
          currentToDoOfToDoList.done === false) ||
        (this.whichRadioState === "doneButton" &&
          currentToDoOfToDoList.done === true)
      ) {
        return true;
      } else {
        return false;
      }
    },
    changeToDoState(toDoToBeChanged, itsIndexInTheToDoList) {
      const toDoSendToBackend = Object.assign({}, toDoToBeChanged);
      toDoSendToBackend.done = !toDoSendToBackend.done;
      fetch(this.urlOfBackend + toDoSendToBackend.id, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(toDoSendToBackend),
      })
        .then((response) => {
          if (response.ok === true) {
            return response.json();
          } else {
            alert("Error: ToDo could not be changed!!!");
          }
        })
        .then((data) => {
          this.toDoList[itsIndexInTheToDoList] = data;
        });
    },
    fetchTheToDos() {
      fetch(this.urlOfBackend)
        .then((response) => {
          if (response.ok === true) {
            return response.json();
          } else {
            alert("Error: ToDos could not be loaded from server!!!");
          }
        })
        .then((data) => {
          this.toDoList = data;
        });
    },
  },
  created() {
    this.fetchTheToDos();
  },
};
</script>

<style scoped>
h2 {
  font-size: 2rem;
  color: blue;
  margin-left: 1rem;
}

ul {
  margin-left: 1rem;
}

label {
  font-size: 1.25rem;
  font-weight: 600;
  margin-right: 0.25rem;
}

.doneToDo {
  text-decoration: line-through;
}
</style>
