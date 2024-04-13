<template>
  <div class="home">
    <h1>ToDo App</h1>
    <RadioButtons @currentRadioState="whichToDosShouldBeDisplayed" />
    <ToDoList :whichRadioState="currentRadioState" />
  </div>
</template>

<script>
import RadioButtons from "@/components/RadioButtons.vue";
import ToDoList from "@/components/ToDoList.vue";

export default {
  data() {
    return {
      currentRadioState: null,
    };
  },
  name: "HomeView",
  components: {
    RadioButtons,
    ToDoList,
  },
  methods: {
    whichToDosShouldBeDisplayed(data) {
      this.currentRadioState = data;
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
