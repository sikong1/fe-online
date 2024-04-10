<template>
  <div class="box">
    <OrgTree :getOrgTreeGetData="getOrgTreeGetData" />
    <UserTable ref="userTableRef" />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import OrgTree from "@/view/member-management/org-tree/index.vue";
import UserTable from "@/view/member-management/user-table/index.vue";

const userTableRef = ref<InstanceType<typeof UserTable>>();

// 获取 UserTable 用户列表
const getOrgTreeGetData = (id: string, detArr: string[]) => {
  userTableRef.value && userTableRef.value?.getSelection(id, detArr);
};
</script>

<style lang="scss" scoped>
.box {
  display: flex;
  justify-content: space-between;
  width: 600px;
  margin: 0 auto;
}
</style>
