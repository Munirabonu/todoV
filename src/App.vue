<script lang="ts">
import VButton from '../src/generic/VButton.vue'
import Filter from './component/Filter.vue'
import { defineComponent, PropType } from 'vue';

export default defineComponent({
  components: {
    Filter,
    VButton
  },

  data() {
    return {
      item: '' as string,
      listItem: JSON.parse(localStorage.getItem('items')) || [],
      editState: false as boolean,
      edit: {} as object
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

    addItem() {
      if (this.item === '') return

      this.listItem.push({ id: Date.now(), text: this.item })
      this.item = ''
    },
    deleteItem(e: object) {
      this.listItem = this.listItem.filter((value: any) => value.id != e)
    },
    editItem(e: object) {
      this.editState = true
      this.edit = e
    },
    saveItem(e: any) {
      e.text = this.edit.text
      this.editState = false
    },
    sort() {
      this.listItem.sort((a: object, b: object) => a.text.localeCompare(b.text))
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
    <div class="add">
      <input type="text" placeholder="New item...." v-model="item" @keydown.enter="addItem">
      <VButton color="primary" @click="addItem">add</VButton>
      <VButton @click="sort" color="success">alphabetically</VButton>
    </div>
    <div class="wrap">
      <div v-for="(list, index) in listItem" :key="index" class="list">
        <div v-if="editState && edit.id === list.id" class="item">
          <input type="text" v-model="edit.text" @keydown.enter="saveItem">
          <VButton @click="saveItem(list)" color="info">save</VButton>
        </div>
        <div v-else class="item">

          <p>{{ index + 1 }} {{ list.text }}</p>
          <VButton @click="deleteItem(list.id)" color="danger" bold>x</VButton>
          <VButton @click="editItem(list)" color="warning">edit</VButton>
        </div>
      </div>
    </div>

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

 input {
  min-width: 65%;
  width: 500px;
  padding: 10px;
  border: none;
  outline: none;
  border-radius: 5px;
}

input::placeholder {
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  font-size: 18px;
}

.wrap {
  width: 50%;
  min-width: 600px;
  max-width: 1000px;
  margin: 20px auto;
  display: grid;
  justify-items: center;
  gap: 25px;
  max-height: 60vh;
  overflow: auto;

}

.list {
  width: 100%;
  background-color: #06758b;
  border-radius: 5px;
  transition: all 1s linear;
}

.item {
  padding: 10px;
  display: grid;
  grid-template-columns: 1fr 10% 10%;
  gap: 10px;
  justify-content: space-between;
  align-items: center;
}

</style>