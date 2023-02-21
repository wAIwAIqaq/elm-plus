<script setup>
import { ref, watch } from "vue";
import { tableDataList, options } from "./shared.js";
const tableData = tableDataList;
const compList = [
  {
    tag: "el-button",
    content: "按钮",
    label: "按钮",
    children: [
      { type: "primary" },
      { type: "success" },
      { type: "info" },
      { type: "warning" },
      { type: "danger" },
    ],
  },
  {
    tag: "el-input",
    content: "输入框",
    label: "输入框",
    children: [{ type: "input" }, { type: "textarea" }],
  },
  {
    tag: "el-select",
    content: "下拉选择框",
    label: "下拉选择框",
    children: [{}],
  },
  {
    tag: "el-table",
    content: "表格",
    label: "表格",
    children: [{ type: "selection" }, { type: "expand" }, { type: "index" }],
  },
];
const labelWidth = ref(100);
const baseBorderRadius = ref(4);
const primaryColor = ref("#409Eff");
const lightColor3 = ref("#79bbff");
const lightfillColor = ref("#f5f7fa");
const placeholderColor = ref("#a8abb2");
const tableHeaderBgColor = ref("#409Eff");
const root = document.documentElement;
watch(baseBorderRadius, () => {
  root.style.setProperty(
    "--el-border-radius-base",
    baseBorderRadius.value + "px"
  );
});
watch(primaryColor, () => {
  root.style.setProperty("--el-color-primary", primaryColor.value);
});
watch(lightColor3, () => {
  root.style.setProperty("--el-color-primary-light-3", lightColor3.value);
});
watch(lightfillColor, () => {
  root.style.setProperty("--el-fill-color-light", lightfillColor.value);
});
watch(placeholderColor, () => {
  root.style.setProperty("--el-text-color-placeholder", placeholderColor.value);
});
watch(tableHeaderBgColor, () => {
  root.style.setProperty(
    "--cms-header-bgcolor-table",
    tableHeaderBgColor.value
  );
});
</script>
<template>
  <div class="grid p-2rem">
    <header class="h-4rem font-bold text-4xl">Element-plus 组件列表</header>
    <div class="flex gap-4rem">
      <el-form label-suffix=":" class="flex-1">
        <el-form-item
          v-for="(comp, index) in compList"
          :label="comp.label"
          :key="index"
          :label-width="labelWidth"
        >
          <div v-if="comp.tag === 'el-input' || comp.tag === 'el-button'">
            <component
              :is="comp.tag"
              v-for="(typeComp, _index) in comp.children"
              :key="_index"
              :type="typeComp.type"
              class="mb-1rem"
              :placeholder="comp.content"
              :data="tableData"
            >
              {{ comp.content }}
            </component>
          </div>
          <div v-else-if="comp.tag === 'el-select'">
            <component
              :is="comp.tag"
              v-for="(typeComp, _index) in comp.children"
              :key="_index"
              :type="typeComp.type"
              class="mb-1rem"
            >
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              />
            </component>
          </div>
          <div v-else-if="comp.tag === 'el-table'">
            <el-table
              :data="tableData"
              v-for="(typeComp, _index) in comp.children"
              class="cms--table__colorful"
              :key="_index"
            >
              <el-table-column :type="typeComp.type" />
              <el-table-column prop="date" label="Date" width="180" />
              <el-table-column prop="name" label="Name" width="180" />
              <el-table-column prop="address" label="Address" />
            </el-table>
          </div>
        </el-form-item>
      </el-form>
      <el-form label-suffix=":" class="flex-1">
        <el-form-item label="表单label-width">
          <el-slider :min="25" v-model="labelWidth" />
        </el-form-item>
        <el-form-item label="基础圆角border-radius">
          <el-slider :min="2" :max="20" v-model="baseBorderRadius" />
        </el-form-item>
        <el-form-item label="主题色primaryColor">
          <el-color-picker v-model="primaryColor" />
        </el-form-item>
        <el-form-item label="高亮色lightColor3">
          <el-color-picker v-model="lightColor3" />
        </el-form-item>
        <el-form-item label="#app外下拉框选项背景色lightfillColor">
          <el-color-picker v-model="lightfillColor" />
        </el-form-item>
        <el-form-item label="占位符颜色placeholderColor">
          <el-color-picker v-model="placeholderColor" />
        </el-form-item>
        <el-form-item label="表格头部背景颜色">
          <el-color-picker v-model="tableHeaderBgColor" />
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<style scoped>
/* form {
  --cms-header-color-table: #f00;
  --cms-header-bgcolor-table: #409Eff;
} */
:deep(.cms--table__colorful th) {
  color: var(--cms-header-color-table);
  background-color: var(--cms-header-bgcolor-table);
}
</style>
