<template>
    <div>
        <input class="input" v-model="input" @keypress.enter="onSubmit_">
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
    
export default defineComponent({
    name: "InputBox",
    props: {
        onSubmit: {type: Function, required: true}
    },
    setup(props) {
        const input = ref("");

        return {
            input,
            onSubmit_() {
                props.onSubmit(input.value);
                input.value = "";
            }
        }
    }
});
</script>

<style lang="scss" scoped>
.input {
    font-size: 25px;
    padding: 5px;
    width: 100%;
}
</style>