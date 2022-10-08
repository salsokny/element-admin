<template>
  <div class="app-container">
    <div>
      <form @submit.prevent="handleSubmit">
        <input
          type="text"
          class="form-control"
          placeholder="Enter yourself..."
          v-model="formData.title"
        />
        <input
          type="text"
          class="form-control"
          placeholder="Enter your name..."
          v-model="formData.author"
        />
        <input
          type="text"
          class="form-control"
          placeholder="Enter your email..."
          v-model="formData.description"
        />
        <button type="submit" class="btn btn-primary">Submit</button>
      </form>
    </div>
    <div>
      <h2>Form Edit Order</h2>
      <el-button @click="visible = true">
        Open Dialog with customized header
      </el-button>
      <el-dialog v-model="visible" :show-close="false">
        <template #header="{ close, titleId, titleClass }">
          <div class="my-header">
            <h4 :id="titleId" :class="titleClass">This is a custom header!</h4>
            <el-button type="danger" @click="close">
              <el-icon class="el-icon--left"><CircleCloseFilled /></el-icon>
              Close
            </el-button>
          </div>
        </template>
        This is dialog content.
      </el-dialog>
    </div>

    <div>
      <el-table :data="dataTable" style="width: 100%">
        <el-table-column prop="id" label="ID" width="80" />
        <el-table-column prop="title" label="Title" width="180" />
        <el-table-column prop="timestamp" label="timestamp" width="180" />
        <el-table-column prop="author" label="Author" width="180" />
        <el-table-column prop="created_at" label="Created At" />
        <el-table-column prop="updated_at" label="Updated At" />
        <el-table-column prop="description" label="Description" width="300" />
        <el-table-column>
          <template #default="scope">
            <!-- Form -->
            <el-button text @click="centerDialogVisible = true"
              >Click to open the Dialog</el-button
            >
            <el-button
              size="small"
              type="danger"
              @click="handleDelete(scope.$index, scope.row)"
              >Delete</el-button
            >
          </template>
        </el-table-column>
      </el-table>
      <pagination
        v-show="total > 0"
        :total="total"
        :page="listQuery.page"
        :limit="listQuery.limit"
        @pagination="getList"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { ElMessage, ElMessageBox } from "element-plus";
import { ref } from "vue";
import { ElButton, ElDialog } from "element-plus";
// import { CircleCloseFilled } from "@element-plus/icons-vue";

const visible = ref(false);
const centerDialogVisible = ref(false);
import axios from "axios";

export default {
  name: "orders",
  components: {},
  data() {
    return {
      formData: {
        title: "",
        author: "",
        description: ""
      },
      tableKey: 0,
      dataTable: [],
      total: 0,
      listLoading: false,
      listQuery: {
        page: 1,
        limit: 20,
        importance: undefined,
        title: undefined,
        type: undefined,
        sort: "+id"
      }
    };
  },

  created() {
    this.getRequest();
  },

  methods: {
    async getRequest() {
      console.log(this.tableKey);
      const options = {
        method: "GET",
        url: "http://192.168.9.119/api/posts"
      };
      const response = await axios(options);
      this.dataTable = response.data.data.items;
      console.log(response.data);
    },
    async handleSubmit() {
      const options = {
        method: "POST",
        url: "http://192.168.9.119/api/posts",
        // url: `http://192.168.9.119/api/posts/update/${id}`,
        data: {
          title: this.formData.title,
          author: this.formData.author,
          description: this.formData.description
        }
      };

      try {
        const response = await axios(options);
      } catch (error) {
        // console.log(error);
        console.log("Erroe" + error.message);
        // console.log(response);
      }
    },
    handleEdit() {
      const options = {
        method: "GET",
        url: "http://192.168.9.119/api/posts"
      };
    }
  }
};
const Opendelete = () => {
  ElMessageBox.confirm("Are you sure you want to delete ?", "Warning", {
    confirmButtonText: "Yes, delete",
    cancelButtonText: "Cancel",
    type: "warning",
    center: true
  })
    .then(() => {
      ElMessage({
        type: "success",
        message: "Delete completed"
      });
    })
    .catch(() => {
      ElMessage({
        type: "info",
        message: "Delete canceled"
      });
    });
};
</script>
