<template>
  <a-select
    v-model:value="value"
    show-search
    placeholder="Select a person"
    style="width: 200px"
    :options="options"
    :filter-option="filterOption"
    @focus="handleFocus"
    @blur="handleBlur"
    @change="handleChange"
  ></a-select>
</template>
<script setup lang="ts">
import pageheader from "../components/PageHeader.vue";
import axios from "axios";
import type { SelectProps } from "ant-design-vue";
import { ref, onMounted } from "vue";
//initial//
const value = ref();
const items = ref([]);
let countItems: any = ref();
const options = ref<SelectProps["options"]>([]);
//interface
interface optionsType {
  value: number;
  label: string;
}
//onMounted
onMounted(() => {
  console.log("mounted");
  getUsers();
});
//getAllUsers
async function getUsers() {
  try {
    const response = await axios.get("http://localhost:5000/users");
    items.value = response.data;
    countItems = response.data.length;
    console.log(`countItems2=${countItems}`);
    for (let i = 0; i < countItems; i++) {
      const newitem: optionsType = {
        value: response.data[i].user_id as number,
        label: `${response.data[i].prefix as string}${
          response.data[i].first_name as string
        } ${response.data[i].last_name as string}`,
      };

      options.value?.push(newitem);
    }
  } catch (err) {
    console.log(err);
  }
}
//Select Options Event
const handleChange = (value: string) => {
  console.log(`selected ${value}`);
};
const handleBlur = () => {
  console.log("blur");
};
const handleFocus = () => {
  console.log("focus");
};
const filterOption = (input: string, option: any) => {
  return option.label.toLowerCase().indexOf(input.toLowerCase()) >= 0;
};
</script>

<style></style>
