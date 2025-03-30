<template>
  <form
    class="flex items-center justify-between bg-white rounded-md mb-5"
    @submit.prevent="addTask"
  >
    <input
      type="text"
      placeholder="Add a new task"
      required
      class="flex-1 font-medium border-none outline-none bg-transparent p-[10px]"
      v-model="task"
    />
    <div dir="rtl">
      <button
        class="bg-gray-300 border-none outline-none py-[14px] px-[50px] text-gray-500 font-medium font-display rounded-s-md transition duration-200 ease cursor-pointer hover:bg-blue-500 hover:text-white"
      >
        Add
      </button>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      task: "",
    };
  },
  methods: {
    addTask() {
      // console.log("added task");
      fetch("http://localhost:3000/todos", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          task: this.task,
          complete: false,
        }),
      })
        .then((response) => response.json())
        .then((newTask) => {
          this.$emit("add", newTask); // Emit the new task to the parent
          this.task = ""; // Clear the input field
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
