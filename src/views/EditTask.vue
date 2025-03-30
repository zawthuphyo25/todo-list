<template>
  <div class="w-full h-screen bg-gray-200">
    <div class="container mx-auto flex flex-col items-center h-full">
      <h1
        class="absolute top-30 text-4xl font-bold text-slate-950 font-display tracking-wider"
      >
        Edit Task
      </h1>

      <form
        class="relative top-60 w-[500px] h-auto bg-slate-950 rounded-lg shadow-lg p-10 flex flex-col gap-10 text-center"
        @submit.prevent="updateTask"
      >
        <input
          type="text"
          placeholder="Edit your task"
          class="font-medium border-none outline-none bg-white rounded-md p-[10px]"
          v-model="task"
        />
        <div>
          <button
            class="bg-gray-300 border-none outline-none py-[14px] px-[50px] text-gray-500 font-medium font-display rounded-md transition duration-200 ease cursor-pointer hover:bg-blue-500 hover:text-white"
          >
            Update
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      task: "",
    };
  },
  methods: {
    updateTask() {
      fetch("http://localhost:3000/todos/" + this.id, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ task: this.task }),
      })
        .then(() => {
          this.$router.push("/");
        })
        .catch((error) => {
          console.error("Error updating task:", error);
        });
    },
  },
  mounted() {
    fetch("http://localhost:3000/todos/" + this.id)
      .then((response) => response.json())
      .then((data) => {
        this.task = data.task;
      })
      .catch((error) => {
        console.error("Error fetching task:", error);
      });
  },
};
</script>
