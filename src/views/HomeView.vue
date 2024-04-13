<template>
  <div class="home">
    <h1>ToDo App</h1>
    <RadioButtons @currentRadioState="whichToDosShouldBeDisplayed" />
    <NewToDoComponent
      :theToDoList="toDoList"
      @updatedToDoList="updateTheToDoList"
    />
    <ToDoList
      :whichRadioState="currentRadioState"
      @sendToDoList="hereIsTheToDoList"
    />
  </div>
</template>

<script>
import RadioButtons from "@/components/RadioButtons.vue";
import NewToDoComponent from "@/components/NewToDoComponent.vue";
import ToDoList from "@/components/ToDoList.vue";

export default {
  data() {
    return {
      currentRadioState: null,
      toDoList: null,
    };
  },
  name: "HomeView",
  components: {
    RadioButtons,
    NewToDoComponent,
    ToDoList,
  },
  methods: {
    whichToDosShouldBeDisplayed(data) {
      this.currentRadioState = data;
    },
    hereIsTheToDoList(data) {
      this.toDoList = data;
    },
    updateTheToDoList(data) {
      this.toDoList = data;
    },
  },
};
</script>

<style>
.home {
  padding-inline: 3rem;
}

h1 {
  color: brown;
  font-size: 3rem;
}
</style>
