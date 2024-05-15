<template>
  <el-pagination
    small
    background
    layout="prev, pager, next"
    :total="props.totalCharacters"
    :page-size="pageSize"
    @current-change="handlePageChange"
  />
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  totalCharacters: Number,
});

const emit = defineEmits(["page-change"]);
const pageSize = 6;

function handlePageChange(newPage) {
  emit("page-change", newPage);
}
</script>

<style>
.el-pagination {
  display: flex;
  justify-content: center;
  margin: 10px 0;
}
</style>
