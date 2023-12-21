<script lang="ts">
import { defineComponent } from 'vue';
import VButton from '@/generic/VButton.vue';
export default defineComponent({
    components: {
        VButton
    },
    props: {
        list: {
            type: Array,
            default: () => [],
        },
    },
    methods: {
        sort() {
            if (Array.isArray(this.list)) {
                // Type assertion: 'as' is used to tell TypeScript that this is an array
                const sortedList = [...this.$props.list].sort((a, b) => {
                    // Type assertion: 'as string' is used to tell TypeScript that 'text' is a string
                    // this.listItem.sort((a: object, b: object) => a.text.localeCompare(b.text))
                    return (a.text as string).localeCompare(b.text as string);
                });
                this.$emit("sortItem", sortedList);
            } else {
                console.warn('Invalid list type.'); // or handle it accordingly
            }
        },
    },


})


</script>

<template>
    <VButton @click="sort" color="success">alphabetically</VButton>
</template>

<style></style>