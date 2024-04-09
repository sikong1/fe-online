<template>
  <div class="org-tree">
    <div class="title">部门</div>
    <el-tree style="max-width: 600px" :props="props" :load="loadNode" lazy />
  </div>
</template>
<script setup lang="ts">
import type Node from "element-plus/es/components/tree/src/model/node";
import orgApi, { Org } from "@/api/org";

const propsVal = defineProps({
  getOrgTreeGetData: {
    type: Function,
    default: () => {},
  },
});
const emits = defineEmits([]);

const props = {
  label: "name",
  children: "zones",
  isLeaf: "leaf",
};
const loadNode = async (
  node: Node,
  resolve: (data: Org[]) => void,
  reject: () => void
) => {
  console.log(node);

  // 默认加载根节点
  if (node.level === 0) {
    const data = await getList();
    return resolve(data);
  }

  // 获取子数据
  const id = node.data.id;
  const data = await getList(id);

  // 通知 App.vue 刷新 UserTable 数据
  propsVal.getOrgTreeGetData && propsVal.getOrgTreeGetData(id);

  return resolve(data);
};

// 获取数据
const getList = (parentId = "0"): Promise<Org[]> => {
  return new Promise((resolve, reject) => {
    orgApi
      .query(parentId)
      .then((users) => {
        resolve(users);
      })
      .catch((err) => {
        reject(err);
      });
  });
};
</script>

<style lang="scss" scoped>
.org-tree {
  width: 200px;
  .title {
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 1px solid #eee;
  }
}
</style>
