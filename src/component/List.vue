<script lang="ts">
import { defineComponent } from 'vue';
import VButton from '../generic/VButton.vue'

export default defineComponent({
    components: {
        VButton,
    },
    props: {
        data: {
            type: Array,
            default: () => []
        }
    },
    data() {
        return {
            edit: false,
            editData: {},
            listItem: this.data
        }
    },
    watch: {
        data: {
            handler(newVal) {
                this.listItem = newVal;
            },
            immediate: true,
        },
    },
    methods: {
        deleteItem(e: object) {
            const newData = this.listItem.filter((value: any) => value.id != e)
            this.listItem = newData
            newData && this.$emit("delItem", newData)
        },
        editItem(e: object) {
            this.edit = true
            this.editData = e
        },
        saveItem(e: any) {
            e.text = this.editData.text
            this.edit = false
        },
    },

})


</script>
<template>
    <div class="wrap">
        <div v-for="(list, index) in listItem" :key="index" class="list">
            <div v-if="edit && editData.id === list.id" class="item">
                <input type="text" v-model="editData.text" @keydown.enter="saveItem">
                <VButton @click="saveItem(list)" color="info">save</VButton>
            </div>
            <div v-else class="item">
                <p>{{ index + 1 }} {{ list.text }}</p>
                <VButton @click="deleteItem(list.id)" color="danger" bold>x</VButton>
                <VButton @click="editItem(list)" color="warning">edit</VButton>
            </div>
        </div>
    </div>
</template>
<style>
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