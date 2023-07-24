<template>
  <div>
    <div
      v-for="category in categories"
      :key="category.id"
      @drop="onDrop($event, category.id)"
      class="droppable"
      @dragover.prevent="onDragover"
      @dragenter.prevent="onDragenter"
    >
      <h4>{{ category.title }}</h4>

      <div
        v-for="item in getCurrentCategotyItems(category.id)"
        :key="item.id"
        @dragstart="onDragStart($event, item)"
        class="draggable"
        draggable="true"
      >
        <h5>{{ item.title }}</h5>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "App",
  setup() {
    const items = ref([
      {
        id: 0,
        title: "Audi",
        categoryId: 0,
      },
      {
        id: 1,
        title: "BMW",
        categoryId: 0,
      },
      {
        id: 2,
        title: "Cat",
        categoryId: 1,
      },
      {
        id: 3,
        title: "Dog",
        categoryId: 1,
      },
    ]);

    const categories = ref([
      {
        id: 0,
        title: "Cars",
      },
      {
        id: 1,
        title: "Animals",
      },
    ]);

    const onDragStart = (e, item) => {
      // move, copy, link И др.
      e.dataTransfer.dropEffect = "move";
      // move, copy, link И др.
      e.dataTransfer.effectAllowed = "move";

      e.dataTransfer.setData("itemId", item.id.toString());
      console.log(e, item);
    };

    const onDrop = (e, categoryId) => {
      const itemId = parseInt(e.dataTransfer.getData("itemId"), 10);

      items.value = items.value.map((x) => {
        if (x.id === itemId) x.categoryId = categoryId;

        return x;
      });

      console.log(e, categoryId);
    };

    const getCurrentCategotyItems = (id) => {
      return items.value.filter((item) => item.categoryId === id);
    };

    return {
      items,
      categories,
      onDragStart,
      onDrop,
      getCurrentCategotyItems,
    };
  },
};
</script>

<style>
.droppable {
  margin-bottom: 18px;

  padding: 15px;

  background: rgb(0, 255, 119);

  border-radius: 5px;
}

.droppable h4 {
  color: white;
}

.draggable {
  margin-bottom: 5px;

  padding: 5px;

  background: white;

  border-radius: 5px;
}

.draggable h5 {
  margin: 0;
}
</style>
