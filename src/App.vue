<script setup>
import { ref, computed } from "vue";

const header = ref("Shopping list app");

const editing = ref(false);
const items = ref([
  // { id: 1, label: "10 party hats", purchased: true, highPriority: false },
  // { id: 2, label: "2 board games", purchased: true, highPriority: false },
  // { id: 3, label: "20 cups", purchased: false, highPriority: true },
]);
const reversedItems = computed(() => {
  return [...items.value].reverse();
});
const newItem = ref("");
const newItemHighPriority = ref("low");

const saveItem = () => {
  items.value.push({
    id: items.value.length + 1,
    label: newItem.value,
    highPriority: newItemHighPriority.value,
  });
  newItem.value = "";
  newItemHighPriority.value = "";
};
const doEdit = (e) => {
  editing.value = e;
  newItem.value = "";
  newItemHighPriority.value = "";
};
const togglePurchased = (item) => {
  item.purchased = !item.purchased;
};
</script>

<template>
  <header>
    <h1>{{ header }}</h1>
    <button v-if="editing" @click="doEdit(false)" class="btn btn-cancel">
      Cancel
    </button>
    <button v-else @click="doEdit(true)" class="btn btn-primary">
      Add Item
    </button>
  </header>
  <form @submit.prevent="saveItem" v-if="editing" class="add-item-form">
    <input
      class="item-input"
      v-model.trim="newItem"
      type="text"
      placeholder="Add an item"
    />
    <label>
      <input type="checkbox" v-model="newItemHighPriority" /> High Priority
    </label>
    <button :disabled="newItem.length < 4" class="btn btn-primary">
      Save Item
    </button>
  </form>

  <ul class="list-items">
    <li
      class="list-item"
      v-for="(item, index) in reversedItems"
      @click="togglePurchased(item)"
      :key="item.id"
      :class="{ strikeout: item.purchased, priority: item.highPriority }"
    >
      {{ item.label }}
    </li>
  </ul>

  <p v-if="!items.length" class="Placeholder-for-empty-list">
    Sorry, you have no item in your list! Please add some items.
  </p>
</template>

<style scoped>
.item-input {
  background-color: rgb(239, 239, 239);
  padding: 0.7rem 0.5rem;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  font-family: inherit;
}

.list-items {
  --clr-item-bg: hsl(144, 100%, 32%);
  display: flex;
  gap: 1rem;
  flex-direction: column;
}

.list-item {
  color: var(--clr-item-bg);
  background-color: transparent;
  width: max-content;
  padding: 0.2rem 0.7rem;
  border: 3px solid var(--clr-item-bg);
  border-radius: 0.5rem;
}

.Placeholder-for-empty-list {
  max-width: 30ch;
  font-size: 1rem;
  color: rgb(52, 181, 26);
}
</style>
