<template>
  <el-button type="danger" @click.prevent="deleteTask(index)">Delete</el-button>
</template>

<script>
export default {
  props: ["taskid"],
  emits: ["delete-task"],
  methods: {
    deleteTask() {
      this.$emit("delete-task", this.index);
    }
  }
};
</script>

<style>
</style>