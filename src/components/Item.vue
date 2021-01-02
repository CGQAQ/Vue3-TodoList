<template>
  <div class="container">
    <span :class="{strikethrough: checked}">{{value}}</span>
    <span @click.stop="click_">
    <input 
      type="checkbox" 
      v-bind:checked="checked" 
      >
    </span>
    <button @click="onDelete_">Delete</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType, ref, watch } from "vue";
import InputBox from "./InputBox.vue";

interface ItemPropType {
  value: string,
  checked: boolean,
}

export default defineComponent({
  name: "Item",
  props: {
    value: String,
    checked: Boolean,
    date: Number,
  },
  emits: ["check", "delete"],
  setup(props, ctx) {
    return {
      click_() {
        ctx.emit("check", {date: props.date, checked: !props.checked});
        // props.onCheck(props.index, !props.checked);
      },
      onDelete_() {
        ctx.emit("delete", {date: props.date});
        // props.onDelete(props.index);
      }
    };
  }
});
</script>

<style scoped lang="scss">
.container {
  display: flex;
  align-items: center;
}
span {
  padding: 4px;
  font-size: 3rem;
}

$cbSize: 2em;
input[type='checkbox'] {
  width: $cbSize;
  height: $cbSize;
  margin-left: .5em;
}


button {
  background: hotpink;
  box-shadow: none;
  appearance: none;
  border: none;
  padding: 4px;
  height: 2rem;
  width: 3rem;
  cursor: pointer;
  text-align: start;;
}

.strikethrough {
  text-decoration: line-through;
  color: grey;
}
</style>
