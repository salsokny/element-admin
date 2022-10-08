<template>
  <el-button text @click="dialogVisible = true"
    >click to open the Dialog</el-button
  >

  <el-dialog
    v-model="dialogVisible"
    title="Tips"
    width="30%"
    :before-close="handleClose"
  >
    <span>This is a message</span>
    <template #footer>
      <span class="dialog-footer">
        <form @submit.prevent="handleSubmit">
          <input type="text" v-model="formData.title" />
          <input type="text" v-model="formData.description" />
          <input type="text" v-model="formData.author" />
          <button type="submit">OK</button>
        </form>

        <el-button @click="dialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="dialogVisible = false"
          >Confirm</el-button
        >
      </span>
    </template>
  </el-dialog>

  <div>
    <h1>{{ formData.title }}</h1>
    <h1>{{ formData.description }}</h1>
    <h1>{{ formData.author }}</h1>
  </div>
</template>

<script lang="ts" setup>
import { reactive, ref } from "vue";
import axios from "axios";
import { ElMessageBox } from "element-plus";

const dialogVisible = ref(false);
const formData = reactive({
  title: "test",
  description: "",
  author: ""
});

const handleSubmit = async () => {
  const options = {
    method: "POST",
    url: "http://192.168.9.119/api/posts",
    // url: `http://192.168.9.119/api/posts/update/${id}`,
    data: {
      title: formData.title,
      author: formData.author,
      description: formData.description
    }
  };

  try {
    const response = await axios(options);
    console.log(response);
    dialogVisible.value = false;
  } catch (error) {
    console.log("error" + error.message);
  }
};
</script>
<style scoped>
.dialog-footer button:first-child {
  margin-right: 10px;
}
</style>
