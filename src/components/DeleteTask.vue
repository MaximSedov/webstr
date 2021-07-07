<template>
  <el-button type="danger" @click.prevent="deleteTask(taskid)">Delete</el-button>
</template>

<script>
export default {
  props: ["taskid"],
  emits: ["delete-task"],
  methods: {
    deleteTask() {
      this.$emit("delete-task", this.taskid);
    }
  }
};
</script>

<style>
</style>