<script setup lang="ts">
import { ref, onMounted } from "vue";
import { Key } from "ant-design-vue/lib/_util/type";
import axios from "axios";

//--------Table---------//
interface DataType {
  user_id: Key;
  prefix: String;
  first_name: String;
  last_name: String;
  department: String;
  position: String;
  level: String;
  role: String;
  email: String;
  password: String;
}
//---------[initial]
//duplicate from database
const userData = ref<DataType[]>([]);
//---------[connect to backend]
//onMounted
onMounted(() => {
  console.log("mounted");
  getUsers();
});
async function getUsers() {
  try {
    const response = await axios.get("http://localhost:5000/users");
    userData.value = response.data;
  } catch (err) {
    console.log(err);
  }
}
const selectedRowKeys = ref<Key[]>([]);
const columns = [
  {
    title: "คำนำหน้า",
    dataIndex: `prefix`,
    width: "8%",
    fixed: "left",
    sorter: true,
  },
  {
    title: "ชื่อ",
    dataIndex: `first_name`,
    width: "6%",
    fixed: "left",
    sorter: true,
  },
  {
    title: "นามสกุล",
    dataIndex: `last_name`,
    width: "6%",
    fixed: "left",
    sorter: true,
  },
  {
    title: "กลุ่ม/ฝ่าย",
    dataIndex: "department",
    filters: [
      {
        text: "ผู้บริหาร",
        value: "ผู้บริหาร",
      },
      {
        text: "ฝ่ายบริหารทั่วไป",
        value: "ฝ่ายบริหารทั่วไป",
      },
      {
        text: "กลุ่มบริหารเทคโนโลยีสารสนเทศและการจัดการ",
        value: "กลุ่มบริหารเทคโนโลยีสารสนเทศและการจัดการ",
      },
      {
        text: "กลุ่มคอมพิวเตอร์และเครือข่าย",
        value: "กลุ่มคอมพิวเตอร์และเครือข่าย",
      },
      {
        text: "กลุ่มพัฒนามาตรฐานและบริการคอมพิวเตอร์",
        value: "กลุ่มพัฒนามาตรฐานและบริการคอมพิวเตอร์",
      },
      {
        text: "กลุ่มพัฒนาการบริหารข้อมูล",
        value: "กลุ่มพัฒนาการบริหารข้อมูล",
      },
      {
        text: "กลุ่มพัฒนานวัตกรรมดิจิทัล",
        value: "กลุ่มพัฒนานวัตกรรมดิจิทัล",
      },
    ],
  },
  {
    title: "ตำแหน่ง",
    dataIndex: "position",
  },
  {
    title: "ระดับ",
    dataIndex: "level",
  },
  {
    title: "บทบาท",
    dataIndex: "role",
    width: "10%",
  },
  {
    title: "อีเมลล์",
    dataIndex: "email",
    width: "25%",
  },
  {
    title: "password",
    dataIndex: "password",
    width: "15%",
  },
];
</script>
<template>
  <div class="content">
    <a-button>add</a-button>
    <a-button>edit</a-button>
    <a-button>delete</a-button>
    <!--search-->
    <a-input-search placeholder="input search text" enter-button @search="" />
    <a-table
      :columns="columns"
      :data-source="userData"
      :scroll="{ x: 1500, y: 560 }"
      :row-selection="selectedRowKeys"
    >
      <template #bodyCell="{ column, text, record }">
        <template v-if="column.dataIndex === 'name'"
          >{{ text.first_name }} {{ text.last_name }}</template
        >
      </template>
    </a-table>
  </div>
</template>
