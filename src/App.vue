<template>
  <div class="drop-zone">
    <div
      v-for="item in getList()"
      :key="item.id"
      @drop="onDrop($event, item)"
      @dragenter.prevent
      @dragover.prevent
      @dragleave.prevent
    >
      <div
        class="draggable"
        draggable="true"
        @dragstart="startDrag($event, item)"
      >
        {{ item.title }}
      </div>
    </div>
  </div>
</template>
<script>
import { ref } from "vue";

export default {
  setup() {
    const items = ref([
      { id: 1, title: "Item B", order: 1 },
      { id: 2, title: "Item C", order: 2 },
      { id: 0, title: "Item A", order: 0 },
      { id: 4, title: "Item E", order: 4 },
      { id: 3, title: "Item D", order: 3 },
    ]);

    let dragStartIndex;

    const getList = () => {
      return items.value.sort((a, b) => a.order - b.order);
    };

    const startDrag = (event, item) => {
      dragStartIndex = item.order;
    };

    const onDrop = (event, item) => {
      const dragEndIndex = item.order;
      swapItems(dragStartIndex, dragEndIndex);
    };

    const swapItems = (fromIndex, toIndex) => {
      const itemOne = items.value.filter((i) => i.order == fromIndex);
      const itemTwo = items.value.filter((i) => i.order == toIndex);
      let itemOneOrder = itemOne[0].order;
      let itemTwoOrder = itemTwo[0].order;
      itemOne[0].order = itemTwoOrder;
      itemTwo[0].order = itemOneOrder;
    };

    return {
      getList,
      onDrop,
      startDrag,
    };
  },
};
</script>
<style>
.drop-zone {
  width: 100%;
  margin: 50px auto;
  background-color: #ecf0f1;
  padding: 10px 50px;
}

.draggable {
  background-color: #3498db;
  color: white;
  padding: 5px;
  margin-bottom: 10px;
}
</style>
