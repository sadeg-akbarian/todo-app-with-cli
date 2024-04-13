<template>
  <h2>Add a newToDo!</h2>
  <input type="text" id="newToDo" v-model="newToDoText" />
  <label for="newToDo">Add the ToDo</label>
  <RegularButtonComponent
    :buttonName="'Add the ToDo'"
    :style="{ 'margin-left': '0.5rem' }"
    @click="pushNewToDo()"
  />
  {{ insertTheToDoList() }}
</template>

<script>
import RegularButtonComponent from "@/components/RegularButtonComponent.vue";

export default {
  name: "NewToDoComponent",
  data() {
    return {
      newToDoText: "",
      toDoList: null,
    };
  },
  methods: {
    insertTheToDoList() {
      this.toDoList = this.theToDoList;
    },
    sendListToHomeView() {
      this.$emit("updatedToDoList", this.toDoList);
    },
    pushNewToDo() {
      if (this.newToDoText.length >= 5) {
        const newToDoInLowCase = this.newToDoText.toLowerCase();
        const alreadyExistArray = [];
        for (let toDo of this.theToDoList) {
          const toDoInLowCase = toDo.description.toLowerCase();
          if (newToDoInLowCase === toDoInLowCase) {
            alreadyExistArray.push(true);
          } else {
            alreadyExistArray.push(false);
          }
        }
        const doesItAlreadyExist = alreadyExistArray.includes(true);
        if (doesItAlreadyExist) {
          alert("This ToDo already exists!!!");
        } else {
          const newToDo = {
            description: this.newToDoText,
            done: false,
          };
          fetch("http://localhost:4730/todos", {
            method: "POST",
            headers: { "Content-type": "application/json" },
            body: JSON.stringify(newToDo),
          })
            .then((response) => {
              if (response.ok === true) {
                return response.json();
              } else {
                alert("Error: New ToDo could not be send to the server!!!");
              }
            })
            .then((data) => {
              this.toDoList.push(data);
              this.newToDoText = "";
            });
        }
      } else {
        alert("Please type in at least 5 characters!!!");
        this.newToDoText = "";
      }
    },
  },
  props: {
    theToDoList: Array,
  },
  components: {
    RegularButtonComponent,
  },
  emits: ["updatedToDoList"],
};
</script>

<style scoped>
h2 {
  font-size: 2rem;
  color: green;
  margin-left: 1rem;
}

input {
  margin-left: 2rem;
}

label {
  display: none;
}
</style>
