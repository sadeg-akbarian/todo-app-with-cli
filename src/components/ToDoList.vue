<template>
  <h2>ToDo List</h2>
  <ul></ul>
</template>

<script>
export default {
  data() {
    return {
      urlOfBackend: "http://localhost:4730/todos/",
      toDoList: [],
    };
  },
  name: "ToDoList",
  methods: {
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
          console.log(data);
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
