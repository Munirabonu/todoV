<script lang="ts">
import { defineComponent } from 'vue';
import VButton from '@/generic/VButton.vue';

export default defineComponent({
    data() {
        return {
            item: "" as string,
            listItem: []
        }
    },
    components: {
        VButton
    },
    methods: {
        addItem() {
            if (this.item === '') return
            this.$emit("newItem", { id: Date.now(), text: this.item.trim() })
            this.item = ''
        },   
    }
})

</script>
<template>
    <div class="add">
        <input type="text" placeholder="New item...." v-model="item" @keydown.enter="addItem">
        <VButton color="primary" @click="addItem">add</VButton>
    </div>
</template>
<style>
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
</style>