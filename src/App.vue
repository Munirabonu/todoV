<script lang="ts">
import VButton from '../src/generic/VButton.vue'
import { defineComponent } from 'vue';
import Add from './component/Add.vue';
import Sort from './component/Sort.vue';
import List from './component/List.vue';

export default defineComponent({
  components: {
    VButton,
    Add,
    Sort,
    List,
  },
  data() {
    return {
      item: '' as string,
      listItem: JSON.parse(localStorage.getItem('items')) || [],
    }
  },
  watch: {
    listItem: {
      handler(newValue) {
        localStorage.setItem('items', JSON.stringify(newValue));
      },
      deep: true,
    },
  },
  methods: {
    addItem(value: object) {
      this.listItem.push(value)
    },
    deleteItem(e: object) {
      this.listItem = e
    },
    sort(value: any) {
      this.listItem = value
    }
  },
})
</script>

<template>
  <div class="bg-blue">
    <h1>WORK TO-DOS</h1>
    <h3 class="">Enter text into the input field to add items to your list. </h3>
    <h3>Click the item to mark it as complete. </h3>
    <h3>Click the "X" to remove the item from your list. </h3>
    <Sort :list="listItem" @sortItem="sort" />
    <br>
    <Add @newItem="addItem" />
    <List :data="listItem" @delItem="deleteItem" />
  </div>
</template>

<style scoped>
.bg-blue {
  width: 100%;
  height: 100vh;
  max-width: 100vw;
  min-height: 100vh;
  background-color: #04A1BF;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.add {
  display: flex;
  align-items: center;
  gap: 10px;
}
</style>