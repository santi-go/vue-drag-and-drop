<template>
  <div
    class="drop-zone"
    @drop="onDrop($event)"
    @dragenter.prevent
    @dragover.prevent
  >
    <div
      v-for="item in getList()"
      :key="item.id"
      class="drag-el"
      draggable="true"
      @dragstart="startDrag($event, item)"
    >
      {{ item.title }}
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
    ]); // esto es el mockup de lo que viene de API call

    const listItems = []; //esto deberia ir en data()

    const getList = () => {
      return items.value.sort((a, b) => a.order - b.order);
    };

    const startDrag = (event, item) => {
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("itemID", item.id);
    };

    const onDrop = (event) => {
      const itemId = event.dataTransfer.getData("itemID");
      const item = items.value.find((item) => item.id == itemId);
      const dragEndIndex = item.order;
      item.list = list;
    };

    const swapItems = (fromIndex, toIndex) => {
      const itemOne = listItems[fromIndex].querySelector(".draggable");
      const itemTwo = listItems[toIndex].querySelector(".draggable");

      listItems[fromIndex].appendChild(itemTwo);
      listItems[toIndex].appendChild(itemOne);
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
  min-height: 10px;
}

.drag-el {
  background-color: #3498db;
  color: white;
  padding: 5px;
  margin-bottom: 10px;
}

.drag-el:nth-last-of-type(1) {
  margin-bottom: 0;
}
</style>
