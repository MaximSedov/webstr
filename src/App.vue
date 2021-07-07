<template>
  <el-row class="header">
    <el-col :xs="24" :sm="24" :md="24" :lg="12" :xl="12">
      <div class="header_title">
        <h1>TO DO List</h1>
        <el-button @click="clearLocalStorage">Clear All Local Storage</el-button>
      </div>
      <AddNewTask @add-new-task="newTask" />
      <el-divider></el-divider>
    </el-col>
  </el-row>
  <el-row class="tasks">
    <el-col :xs="24" :sm="24" :md="24" :lg="12" :xl="12">
      <div class="tasks_header">
        <h2>Tasks</h2>
        <el-tag>{{tasks.length}}</el-tag>
      </div>
      <div class="tasks_block" v-for="(task, index) in tasks" :key="task.task">
        <el-checkbox @change="check(index, 'need')">{{task.date + ' ' + task.task}}</el-checkbox>
        <DeleteTask :taskid="index" @delete-task="deleteTask" />
      </div>
    </el-col>
  </el-row>
  <el-row class="completedTasks">
    <el-col :xs="24" :sm="24" :md="24" :lg="12" :xl="12">
      <div class="completedTasks_header">
        <h2>Completed tasks</h2>
        <el-tag type="info">{{completedTasks.length}}</el-tag>
      </div>
      <div class="completedTasks_block" v-for="(task, index) in completedTasks" :key="task.task">
        <el-checkbox checked="checked" @change="check(index, 'complete')">{{task.date + ' ' + task.task}}</el-checkbox>
        <DeleteTask :taskid="index" @delete-task="deleteCompletedTask" />
      </div>
    </el-col>
  </el-row>
</template>

<script>
import AddNewTask from "./components/AddNewTask.vue";
import DeleteTask from "./components/DeleteTask.vue";
export default {
  data() {
    return {
      tasks: [],
      completedTasks: [],
      checked: true
    };
  },
  methods: {
    newTask(data) {
      this.tasks.unshift(data);
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    deleteCompletedTask(index) {
      this.completedTasks.splice(index, 1);
      localStorage.setItem(
        "completedTasks",
        JSON.stringify(this.completedTasks)
      );
      console.log(this.completedTasks);
    },
    check(index, type) {
      if (type === "need") {
        const needTask = this.tasks.splice(index, 1);
        this.completedTasks.push(...needTask);
        localStorage.setItem(
          "completedTasks",
          JSON.stringify(this.completedTasks)
        );
        localStorage.setItem("tasks", JSON.stringify(this.tasks));
      } else {
        const completeTask = this.completedTasks.splice(index, 1);
        this.tasks.push(...completeTask);
        localStorage.setItem(
          "completedTasks",
          JSON.stringify(this.completedTasks)
        );
        localStorage.setItem("tasks", JSON.stringify(this.tasks));
      }
    },
    clearLocalStorage() {
      localStorage.clear();
      this.tasks = [];
      this.completedTasks = [];
    }
  },
  mounted() {
    if (localStorage.getItem("tasks")) {
      this.tasks = JSON.parse(localStorage.getItem("tasks"));
    }
    if (localStorage.getItem("completedTasks")) {
      this.completedTasks = JSON.parse(localStorage.getItem("completedTasks"));
    }
  },

  components: { AddNewTask, DeleteTask }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap");
* {
  margin: 0;
  padding: 0;
  font-family: "Roboto", sans-serif;
}
.header {
  margin: 1rem;
  &_title {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 2rem;
  }
}
.tasks {
  margin: 1rem;
  &_header {
    display: flex;
    justify-content: flex-start;
    margin-bottom: 2rem;
    .el-tag {
      margin-left: 1rem;
    }
  }
  &_block {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
  }
}
.completedTasks {
  margin: 1rem;
  &_header {
    display: flex;
    justify-content: flex-start;
    margin-bottom: 2rem;
    .el-tag {
      margin-left: 1rem;
    }
  }
  &_block {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
    .el-checkbox {
      span {
        text-decoration: line-through;
      }
    }
  }
}
</style>
