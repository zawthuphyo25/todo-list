<template>
  <div
    class="flex justify-between items-center bg-slate-800/50 p-[10px] mb-4 rounded-md"
  >
    <div class="flex items-center gap-2">
      <!-- Complete Feature -->
      <i
        :class="[
          todo.complete
            ? 'ri-checkbox-circle-fill text-blue-500'
            : 'ri-checkbox-blank-circle-line text-white',
          'text-2xl cursor-pointer duration-200 hover:opacity-80',
        ]"
        @click="completeTask"
      ></i>
      <!-- Task -->
      <h3
        :class="[
          'text-white font-medium',
          todo.complete ? 'line-through opacity-50' : '',
        ]"
      >
        {{ todo.task }}
      </h3>
    </div>
    <div class="flex items-center gap-2">
      <!-- Edit Feature -->
      <router-link :to="{ name: 'editTask', params: { id: todo.id } }">
        <i
          class="ri-edit-box-line text-2xl text-blue-500 cursor-pointer duration-200 hover:opacity-80"
        ></i>
      </router-link>
      <!-- Delete Feature -->
      <i
        class="ri-delete-bin-6-line text-2xl text-red-500 cursor-pointer duration-200 hover:opacity-80"
        @click="deleteTask"
      ></i>
    </div>
  </div>
</template>

<script>
export default {
  props: ["todo"],
  data() {
    return {
      api: "http://localhost:3000/todos/",
    };
  },
  methods: {
    deleteTask() {
      let deleteRoute = this.api + this.todo.id;
      fetch(deleteRoute, {
        method: "DELETE",
      })
        .then(() => {
          this.$emit("delete", this.todo.id);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    completeTask() {
      let completeRoute = this.api + this.todo.id;
      fetch(completeRoute, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          complete: !this.todo.complete,
        }),
      })
        .then(() => {
          this.$emit("complete", this.todo.id);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
