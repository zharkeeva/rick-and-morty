<template>
  <div class="filtration-panel">
    <el-form @submit.prevent="applyFilter">
      <el-form-item label="Name">
        <el-input
          v-model="filters.name"
          placeholder="Enter character name"
          clearable
        ></el-input>
      </el-form-item>
      <el-form-item label="Status">
        <el-select
          v-model="filters.status"
          placeholder="Select status"
          clearable
        >
          <el-option label="Alive" value="Alive"></el-option>
          <el-option label="Dead" value="Dead"></el-option>
          <el-option label="unknown" value="unknown"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="applyFilter" native-type="submit"
          >Применить</el-button
        >
      </el-form-item>
    </el-form>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";
import {
  ElForm,
  ElFormItem,
  ElInput,
  ElSelect,
  ElOption,
  ElButton,
} from "element-plus";

const filters = ref({
  name: "",
  status: "",
});

const emit = defineEmits(["filter"]);

async function applyFilter() {
  await new Promise((resolve) => setTimeout(resolve, 1000));
  emit("filter", filters.value);
}
</script>

<style scoped>
.filtration-panel {
  padding: 10px;
  background-color: var(--main-text);
}

.el-form {
  max-width: 400px;
}

.el-form-item {
  margin-bottom: 10px;
  width: 100%;
}

::v-deep .el-input,
::v-deep .el-select {
  width: 100%;
}

::v-deep .el-input__inner,
::v-deep .el-select__placeholder {
  font-size: smaller;
}
</style>
