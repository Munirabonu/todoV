<script lang="ts">
import { defineComponent, type PropType } from 'vue';
export type VInputType = 'number' | 'text'

export default defineComponent({
    props: {
        type: {
            type: String as PropType<VInputType>,
            default: "text",
            validator: (value: VInputType) => {
                // The value must match one of these strings
                if (
                    [
                        undefined,
                        'text',
                        'number',
                    ].indexOf(value) === -1
                ) {
                    console.warn(
                        `VInput: invalid "${value}" type. Should be number,text or undefined`
                    )
                    return false
                }

                return true
            },

        }
    }
})
</script>

<template>
    <input :type="type">
</template>

<style scoped>
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