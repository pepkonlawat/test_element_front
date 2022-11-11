<!-- ! I use for loop for resturture data and change property name. I think there are better way to deal with it. -->
<script setup lang="ts">
import axios from "axios";
import { SelectProps } from "ant-design-vue";
import { ref, onMounted, watch, Ref } from "vue";
import { object } from "vue-types";
//---------[interface]
interface optionsType {
  value: number;
  label: string;
}
interface dataType {
  user_id: number;
  name: string;
  department: string;
  position: string;
  level: string;
}

//---------[initial]
//duplicate from database
const userData = ref<dataType[]>([]);
//basket
const UnSelectedData = ref<dataType[]>([]);
const SelectedData = ref<dataType[]>([]);
//option
const options = ref<SelectProps["options"]>([]);
//selected option
const selectedOption = ref();
//checkbox plainOpitons
const plainOpitons = ref([
  "ฝ่ายบริหารทั่วไป",
  "กลุ่มคอมพิวเตอร์และเครือข่าย",
  "กลุ่มพัฒนาการบริหารข้อมูล",
  "ผู้บริหาร",
  "กลุ่มบริหารเทคโนโลยีสารสนเทศและการจัดการ",
  "กลุ่มพัฒนามาตรฐานและบริการคอมพิวเตอร์",
  "กลุ่มพัฒนานวัตกรรมดิจิทัล",
]);
//checkbox checkAll
const state = ref({
  indeterminate: true,
  checkAll: false,
  checkedList: [],
});
//table columns
const columns = [
  {
    title: "เจ้าหน้าที่",
    dataIndex: "name",
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
    for (let i = 0; i < response.data.length; i++) {
      userData.value.push({
        user_id: response.data[i].user_id,
        name: `${response.data[i].prefix}${response.data[i].first_name} ${response.data[i].last_name}`,
        department: response.data[i].department,
        position: response.data[i].position,
        level: response.data[i].level,
      });
    }
    setInitial(); //เอามาไว้ตรงนี้เพราะให้ initial เมื่อ userData รับค่าจาก backend แล้ว
  } catch (err) {
    console.log(err);
  }
}

//---------[options]
//getOptions --> initial,checkbox,click add user
const getOptions = () => {
  console.log(UnSelectedData.value.length);
  for (let i = 0; i < UnSelectedData.value.length; i++) {
    (options.value as any).push({
      value: UnSelectedData.value[i].user_id,
      label: UnSelectedData.value[i].name,
    });
  }
  console.log("getOptions");
};
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
const selected = () => {
  UnSelectedData.value.find((x) => x.user_id === selectedOption.value.user_id);
  console.log(selectedOption.value.user_id);
};
//---------[checkbox]
const onCheckAllChange = (e: any) => {
  console.log("onCheckAllChange");
  Object.assign(state.value, {
    checkedList: e.target.checked ? plainOpitons.value : [],
    indeterminate: false,
  });
  console.log(e.target.checked);
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

//---------[table]
//setUnselectedData: initial and reset data
const setInitial = () => {
  console.log("setInitial");
  SelectedData.value = [];
  UnSelectedData.value = userData.value;
  console.log(userData);
  console.log(`userData length : ${userData.value.length}`);
  console.log(UnSelectedData);
  console.log(`UnSelectedData length : ${UnSelectedData.value.length}`);
  getOptions();
};
//insertItem--->orgBasket = Origin Basket and desBasket = Destination Basket
const transferData = (orgBasket: any, desBasket: any, userID: number) => {
  const select = ref([]);
  //find Index
  const index = orgBasket.findIndex((object: any) => {
    return object.user_id == userID;
  });
  if (index > -1) {
    // only splice array when item is found
    select.value = orgBasket.splice(index, 1); // 2nd parameter means remove one item only
  }
  console.log(select);
  desBasket.push(select);
  // array = [2, 9]
  console.log(orgBasket);
  console.log("transferData");
};
</script>
<template>
  <!--selection-->
  <div>
    <a-select
      v-model:value="selectedOption"
      show-search
      placeholder="Select a person"
      style="width: 200px"
      :options="options"
      :filter-option="filterOption"
      @focus="handleFocus"
      @blur="handleBlur"
      @change="handleChange"
    />
    <a-button @click="selected">เพิ่มรายชื่อ</a-button>
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
      :dataSource="UnSelectedData"
      :columns="columns"
      size="small"
      :scroll="{ x: 1200, y: 200 }"
    />
  </div>
</template>
<style></style>
