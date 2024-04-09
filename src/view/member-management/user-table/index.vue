<template>
  <div class="user-table">
    <div class="dep">{{ dep }}</div>
    <div class="search">
      <el-select
        class="select"
        v-model="id"
        filterable
        remote
        placeholder="Select"
        style="width: 240px"
        @change="change"
        :remote-method="remoteMethod"
      >
        <el-option
          v-for="item in option"
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
      <el-table-column prop="name" label="用户名" />
    </el-table>
  </div>
</template>
<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import { ElTable } from "element-plus";
import userApi, { User } from "@/api/user";
import { debounce } from "@/utils/index";

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

const option = ref<User[]>([]);
const remoteMethod = (query: string) => {
  const getOption = async () => {
    if (query) {
      option.value = await userApi.query({
        name: query,
        orgId: orgId.value,
      });
    } else {
      option.value = [];
    }
  };
  debounce(getOption)();
};

// 改变选择
const change = (val: string) => {
  id.value = val;
  const item = option.value.find((item) => item.id === val);
  if (item) {
    getData({
      orgId: orgId.value,
      name: item.name,
      id: item.id,
    });
    option.value = [item];
  }
};

// 外部树形组件调用
const orgId = ref("0");
const dep = ref("");
function getSelection(id: string, detArr: string[]) {
  orgId.value = id;
  dep.value = detArr.join("/");

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

  .dep {
    margin-bottom: 20px;
  }
}
</style>
