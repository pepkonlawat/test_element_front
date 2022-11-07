<script setup lang="ts">
import axios from "axios";
import { onMounted, onUnmounted, onUpdated, ref } from "vue";

// interface //
interface itemType {
  product_id: String;
  product_name: String;
  product_price: String;
}
interface optionsType {
  value: number;
  label: string;
}
// variables //
const value = ref();
const items = ref<itemType[]>([]);
const options: optionsType[] = [];
let countItems: any;
//onMounted lifecycle hook//

onMounted(() => {
  console.log("mounted");
  getUsers();
  console.log(options);
});

async function getUsers() {
  try {
    const response = await axios.get("http://localhost:5000/products");
    items.value = response.data;
    countItems = response.data.length;
    console.log(`countItems2=${countItems}`);
    for (let i = 0; i < countItems; i++) {
      const newitem: optionsType = {
        value: response.data[i].product_id as any,
        label: response.data[i].product_name as any,
      };
      (options as any).push(newitem);
    }
  } catch (err) {
    console.log(err);
  }
}
</script>
<template>
  {{ options }}<br />

  <a-select
    v-model:value="value"
    show-search
    placeholder="Select a person"
    style="width: 200px"
    :options="options"
  ></a-select>
</template>
