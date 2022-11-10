<script setup lang="ts">
import axios from "axios";
import type { SelectProps } from "ant-design-vue";
import { ref, onMounted, watch } from "vue";
//initial//
const UnSelectedData = ref([]);
const SelectedData = ref<dataType[]>([]);
const value = ref();
const items = ref([]);
const countItems = ref();
const options = ref<SelectProps["options"]>([]);
//interface//
interface optionsType {
  value: number;
  label: string;
}
interface dataType {
  first_name: string;
  department: string;
  position: string;
  level: string;
}
//onMounted//
onMounted(() => {
  console.log("mounted");
  getUsers();
});
//getAllUsers
async function getUsers() {
  try {
    const response = await axios.get("http://localhost:5000/users");
    items.value = response.data;
    countItems.value = response.data.length;
    console.log(`countItems2=${countItems.value}`);
    for (let i = 0; i < countItems.value; i++) {
      const newitem: optionsType = {
        value: response.data[i].user_id as number,
        label: `${response.data[i].prefix as string}${
          response.data[i].first_name as string
        } ${response.data[i].last_name as string}`,
      };

      options.value?.push(newitem);
    }
    for (let i = 0; i < items.value.length; i++) {
      // console.log(`items=${(items.value[i] as any).first_name}`);
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
///////////////////////////////////
const plainOpitons = ref([
  "ฝ่ายบริหารทั่วไป",
  "กลุ่มคอมพิวเตอร์และเครือข่าย",
  "กลุ่มพัฒนาการบริหารข้อมูล",
  "ผู้บริหาร",
  "กลุ่มบริหารเทคโนโลยีสารสนเทศและการจัดการ",
  "กลุ่มพัฒนามาตรฐานและบริการคอมพิวเตอร์",
  "กลุ่มพัฒนานวัตกรรมดิจิทัล",
]);
const state = ref({
  indeterminate: true,
  checkAll: false,
  checkedList: [],
});
const onCheckAllChange = (e: any) => {
  console.log("onCheckAllChange");
  Object.assign(state.value, {
    checkedList: e.target.checked ? plainOpitons.value : [],
    indeterminate: false,
  });
  console.log(e.target.checked);
  insertData();
};
//ทำงานก็ต่อเมื่อตัวแปรที่เรา watch มีการเปลี่ยนแปลง//
watch(
  () => state.value.checkedList,
  (val) => {
    state.value.indeterminate =
      !!val.length && val.length < plainOpitons.value.length;
    state.value.checkAll = val.length === plainOpitons.value.length;
  }
);
///////////////////////table///////////////////
const columns = [
  {
    title: "การจัดการ",
    width: "6%",
    fixed: true,
  },
  {
    title: "เจ้าหน้าที่",
    dataIndex: "first_name",
    width: "13%",
    fixed: true,
  },
  {
    title: "กลุ่ม/ฝ่าย",
    dataIndex: "department",
    width: "20%",
  },
  {
    title: "ตำแหน่ง",
    dataIndex: "position",
    width: "20%",
  },
  {
    title: "ระดับ",
    dataIndex: "level",
    width: "10%",
  },
];
const insertData = () => {
  console.log(`countItems3=${countItems.value}`);
  for (let i = 0; i < countItems.value; i++) {
    const newitem: dataType = {
      first_name: (items.value[i] as any).first_name,
      department: (items.value[i] as any).department,
      position: (items.value[i] as any).position,
      level: (items.value[i] as any).level,
    };
    SelectedData.value.push(newitem);
  }
};
</script>
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
  ></a-select
  ><br />
  <a-space>
    <a-button>เพิ่มรายชื่อ</a-button>
    <a-button>reset </a-button>
  </a-space>

  <!--checkbox เลือกฝ่าย-->

  <a-checkbox
    v-model:checked="state.checkAll"
    :indeterminate="state.indeterminate"
    @change="onCheckAllChange"
    >ทั้งหมด</a-checkbox
  ><br />
  <a-checkbox-group v-model:value="state.checkedList" :options="plainOpitons" />

  <div id="table">
    <a-table
      :dataSource="SelectedData"
      :columns="columns"
      size="small"
      :scroll="{ x: 1200, y: 200 }"
    ></a-table>
  </div>
</template>
<style></style>
