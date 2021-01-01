<template>
  <InputBox :onSubmit="inputOnSubmit"/>
  <ul>
    <Item 
      v-for="item in data" 
      :key="item.date"
      :date="item.date"
      :value="item.value"
      :checked="item.checked"
      @check="onCheck"
      @delete="onDelete"
      />
  </ul>
</template>

<script lang="ts">
import { defineComponent, ref, Ref, watch} from "vue";
import InputBox from "./components/InputBox.vue";
import Item from "./components/Item.vue";

interface ItemDataType {
  date: number,
  value: string,
  checked: boolean,
}

export default defineComponent({
  name: "App",
  components: {InputBox, Item},
  setup() {
    const data: Ref<ItemDataType[]> = ref([]);
    const findAndDo = (date: number, fn: (item: ItemDataType, index: number)=>void) => {
      for(let index = 0; index < data.value.length; index++){
        const item = data.value[index];
        if(item.date === date){
          fn(item, index);
          return;
        }
      }
    }

    return {
      data,
      inputOnSubmit(inputString: string) {
        data.value.push({date: parseInt(Date.now().toString()), value: inputString, checked: false});
      },
      onCheck({date, checked}: {date: number, checked: boolean}) {
        const massage = (bool: boolean) => data.value.filter(it => it.checked === bool).sort((a,b)=>a.date-b.date);
        findAndDo(date, (item) => {
          item.checked = checked;
          data.value = [...massage(false), ...massage(true)];
        });
      },
      onDelete({date}: {date: number}) {
        findAndDo(date, (_, index) => data.value.splice(index, 1));
      }
    }
  }
});
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

:root{
  padding: 8px 16px;
}
</style>
