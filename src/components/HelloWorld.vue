<script setup lang="ts">
import axios from "axios";
import { SelectProps } from "ant-design-vue";
import { ref, onMounted, watch } from "vue";
//---------[interface]
interface optionsType {
  value: number;
  label: string;
}
interface dataType {
  user_id: number;
  first_name: string;
  department: string;
  position: string;
  level: string;
}

//---------[initial]
//duplicate from database
const userData = ref([]);
//basket
const UnSelectedData = ref<dataType[]>([]);
const SelectedData = ref<dataType[]>([]);
//option
const options = ref<SelectProps["options"]>([]);

//---------[connect to backend]
//onMounted
onMounted(() => {
  console.log("mounted");
  getUsers();
});
//getAllUsers
async function getUsers() {
  try {
    const response = await axios.get("http://localhost:5000/users");
    userData.value = response.data;
  } catch (err) {
    console.log(err);
  }
}

//---------[method]
//setUnselectedData: initial and reset data
const setInitial = () => {
  SelectedData.value = [];
  UnSelectedData.value = [];
};
//getOptions
const getOptions = () => {
  console.log("getOptions");
};
//dropItem
const dropItem = (basket: object, userID: number) => {
  console.log("dropItem");
};
//insertItem
const insertItem = (basket: object, userID: number) => {
  console.log("insertItem");
};
</script>
<template>
  <!--selection-->
  <div>
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
    />
    <a-button>เพิ่มรายชื่อ</a-button>
  </div>
  <!--checkbox-->
  <div>
    <a-checkbox
      v-model:checked="state.checkAll"
      :indeterminate="state.indeterminate"
      @change="onCheckAllChange"
    >
      ทั้งหมด
    </a-checkbox>
    <a-checkbox-group
      v-model:value="state.checkedList"
      :options="plainOpitons"
    />
  </div>
  <!--table-->
  <div>
    <a-table
      :dataSource="SelectedData"
      :columns="columns"
      size="small"
      :scroll="{ x: 1200, y: 200 }"
    />
  </div>
</template>
<style></style>
