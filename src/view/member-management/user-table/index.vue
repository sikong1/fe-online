<template>
  <div class="user-table">
    <div class="search">
      <el-select
        class="select"
        v-model="id"
        filterable
        placeholder="Select"
        style="width: 240px"
        @change="change"
      >
        <el-option
          v-for="item in tableData"
          :key="item.id"
          :label="item.name"
          :value="item.id"
        />
      </el-select>
      <span>登录状态 {{ total }} 个成员</span>
    </div>
    <el-table
      ref="multipleTableRef"
      :data="tableData"
      border
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="55" />
      <el-table-column prop="id" label="id" width="200" />
      <el-table-column prop="name" label="姓名" />
    </el-table>
  </div>
</template>
<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import { ElTable } from "element-plus";
import userApi, { User } from "@/api/user";

onMounted(() => {
  getData({
    orgId: "0",
  });
});

const multipleTableRef = ref<InstanceType<typeof ElTable>>();
const multipleSelection = ref<User[]>([]);

const tableData = ref<User[]>([]);
const id = ref("");

const total = computed(() => {
  return tableData.value.length;
});

// 点击 selection 选中
const handleSelectionChange = (val: User[]) => {
  multipleSelection.value = val;
};

// 获取数据
const getData = async (params: Params) => {
  const users = await userApi.query(params);
  tableData.value = users;
};

// 改变选择
const change = (val: string) => {
  id.value = val;
  const item = tableData.value.find((item) => item.id === val);
  item &&
    getData({
      orgId: orgId.value,
      name: item.name,
      id: item.id,
    });
};

// 外部树形组件调用
const orgId = ref("0");
function getSelection(id: string) {
  orgId.value = id;

  // 树形组件点击
  getData({
    orgId: id,
  });
}

defineExpose({
  getSelection,
});
</script>

<style lang="scss" scoped>
.user-table {
  margin-left: 20px;

  .search {
    width: 100%;
    display: flex;
    justify-content: space-between;
    padding-bottom: 10px;

    .select {
      flex: 0.9;
    }
  }
}
</style>
