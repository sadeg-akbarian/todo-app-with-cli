<template>
  <h2>ToDo List</h2>
  <RegularButtonComponent
    :buttonName="'Delete Done ToDos'"
    @click="deleteDoneToDos()"
    :style="{ 'margin-left': '2rem' }"
  />
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
import RegularButtonComponent from "@/components/RegularButtonComponent.vue";

export default {
  data() {
    return {
      urlOfBackend: "http://localhost:4730/todos/",
      toDoList: [],
    };
  },
  name: "ToDoList",
  components: {
    RegularButtonComponent,
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
          this.sendToHomeView();
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
          this.sendToHomeView();
        });
    },
    deleteDoneToDos() {
      const listOfDoneToDos = [];
      for (let toDo of this.toDoList) {
        if (toDo.done === true) {
          listOfDoneToDos.push(toDo);
        }
      }
      for (let i = 0; i < listOfDoneToDos.length; i++) {
        fetch(this.urlOfBackend + listOfDoneToDos[i].id, {
          method: "DELETE",
        })
          .then((response) => {
            if (response.ok === true) {
              return response.json();
            } else {
              alert(
                `The ToDo: '${listOfDoneToDos[i].description}' could not be deleted!!!`
              );
            }
          })
          .then(() => {
            if (i === listOfDoneToDos.length - 1) {
              this.fetchTheToDos();
            }
          });
      }
    },
    sendToHomeView() {
      this.$emit("sendToDoList", this.toDoList);
    },
  },
  emits: ["sendToDoList"],
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
  margin-top: 4rem;
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
