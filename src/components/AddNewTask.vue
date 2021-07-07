<template>
  <el-form :rules="rules" ref="taskForm" :model="taskForm">
    <el-row>
      <el-col :xs="24" :sm="24" :md="7" :lg="7" :xl="7">
        <el-form-item label="Date" prop="date">
          <el-date-picker
            format="DD-MM-YYYY"
            value-format="DD-MM-YYYY"
            type="date"
            :disabled-date="disabledDate"
            placeholder="Pick a date"
            v-model="taskForm.date"
            style="width: 100%;"
          ></el-date-picker>
        </el-form-item>
      </el-col>
      <el-col :xs="0" :sm="0" :md="1" :lg="1" :xl="1"></el-col>
      <el-col :xs="24" :sm="24" :md="11" :lg="11" :xl="11">
        <el-form-item label="Task" prop="task">
          <el-input maxlength="20" show-word-limit v-model="taskForm.task" @keypress.enter="newTask('taskForm')"></el-input>
        </el-form-item>
      </el-col>
      <el-col :xs="0" :sm="0" :md="1" :lg="1" :xl="1"></el-col>
      <el-col :xs="24" :sm="24" :md="4" :lg="4" :xl="4">
        <el-form-item>
          <el-button type="primary" style="width:100%" @click="newTask('taskForm')">Add new task</el-button>
        </el-form-item>
      </el-col>
    </el-row>
  </el-form>
</template>

<script>
export default {
  emits: ["add-new-task"],
  data() {
    return {
      taskForm: {
        task: "",
        date: ""
      },
      rules: {
        task: [
          { required: true, message: "Please input Task text", trigger: "blur" },
          { min: 3, max: 20, message: 'Length should be 3 to 20', trigger: 'blur' }
        ],
        date: [
          { required: true, message: "Please input Task date", trigger: "blur" }
        ]
      },
      disabledDate(time) {
        var date = new Date();
        return time.getTime() <= date.setDate(date.getDate() - 1);
      }
    };
  },
  methods: {
    newTask(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          const formData = {
            task: this.taskForm.task,
            date: this.taskForm.date
          };
          this.$emit("add-new-task", formData);
          this.$refs[formName].resetFields();
        } else {
          return false;
        }
      });
    }
  }
};
</script>

<style lang="scss">
</style>