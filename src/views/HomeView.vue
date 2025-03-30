<template>
  <div class="home">
    <div class="w-full h-screen bg-gray-200">
      <div class="container mx-auto flex flex-col items-center h-full">
        <div
          class="absolute top-30 w-[500px] h-[75vh] bg-slate-950 rounded-lg shadow-lg p-10"
        >
          <!-- Header -->
          <h1
            class="text-4xl font-bold text-blue-500 font-display tracking-wide text-center"
          >
            To-do List
          </h1>

          <div class="mt-10">
            <!-- Add Task -->
            <AddTask @add="addTaskToList"></AddTask>
            <!-- Tasks -->
            <div class="overflow-y-auto h-[50vh]">
              <div v-for="todo in todos" :key="todo.id">
                <SingleTask
                  :todo="todo"
                  @delete="deleteTask"
                  @complete="completeTask"
                ></SingleTask>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SingleTask from "../components/SingleTask";
import AddTask from "../components/AddTask";
export default {
  name: "HomeView",
  components: {
    SingleTask,
    AddTask,
  },
  data() {
    return {
      todos: [],
    };
  },
  methods: {
    deleteTask(id) {
      // console.log(id);
      this.todos = this.todos.filter((todo) => {
        return todo.id != id;
      });
    },
    completeTask(id) {
      // console.log(id);
      let findTask = this.todos.find((todo) => {
        return todo.id === id;
      });
      findTask.complete = !findTask.complete;
    },
    addTaskToList(newTask) {
      this.todos.push(newTask); // Add the new task to the list
    },
  },
  mounted() {
    fetch("http://localhost:3000/todos")
      .then((response) => response.json())
      .then((data) => {
        this.todos = data;
        // console.log(this.todos);
      })
      .catch((error) => {
        console.log(error);
      });
  },
};
</script>
