<script lang="ts">
interface ListItem {
  id: number;
  text: string;
}

export default {
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
      this.listItem.sort((a:object, b:object) => a.text.localeCompare(b.text))
    }
  },
}


</script>

<template>
  <div class="bg-blue">
    <h1>WORK TO-DOS</h1>
    <h3 class="">Enter text into the input field to add items to your list. </h3>
    <h3>Click the item to mark it as complete. </h3>
    <h3>Click the "X" to remove the item from your list. </h3>

    <div class="add">
      <input type="text" placeholder="New item...." v-model="item" @keydown.enter="addItem">
      <button @click="addItem">add</button>
      <button @click="sort">alphabetically</button>
    </div>
    <table>
      <div v-for="(list, index) in listItem" :key="index">

        <tr v-if="editState && edit.id === list.id">
          <td><input type="text" v-model="edit.text" @keydown.enter="saveItem"></td>
          <td><button @click="saveItem(list)">save</button></td>
        </tr>
        <tr v-else>
          <td>{{ index + 1 }}</td>
          <td>{{ list.text }}</td>
          <td><button @click="deleteItem(list.id)">delete</button></td>
          <td><button @click="editItem(list)">edit</button></td>
        </tr>
      </div>
    </table>

  </div>
</template>
<style>
.bg-blue {
  width: 100%;
  height: 100vh;
  max-width: 100vw;
  min-height: 100vh;
  background-color: #04A1BF;
  text-align: center;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  
}

.add {
  display: flex;
  height: 30px;
}

.bg-blue .add input {
  min-width: 65%;
  width: 500px;
  padding: 5px;
  border: none;
  outline: none;
  border-radius: 5px;
}

input::placeholder {
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  font-size: 18px;
}

button {
  border: none;
  padding: 5px 15px;
  border-radius: 5px;
  color: #04A1BF;
  background-color: #025F70;
  transition: all 0.75s ease;
  -webkit-transition: all 0.75s ease;
}
</style>