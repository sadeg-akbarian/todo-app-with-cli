<template>
  <div class="home">
    <h1>ToDo App</h1>
    <RadioButtons @currentRadioState="whichToDosShouldBeDisplayed" />
    <ToDoList />
  </div>
</template>

<script>
import RadioButtons from "@/components/RadioButtons.vue";
import ToDoList from "@/components/ToDoList.vue";

export default {
  name: "HomeView",
  components: {
    RadioButtons,
    ToDoList,
  },
  methods: {
    whichToDosShouldBeDisplayed(data) {
      console.log(data);
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
