<script setup lang="ts">
import { ref, watch } from "vue";
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
</script>
<template>
  <!--checkbox เลือกฝ่าย-->

  <a-checkbox
    v-model:checked="state.checkAll"
    :indeterminate="state.indeterminate"
    @change="onCheckAllChange"
    >ทั้งหมด</a-checkbox
  ><br />
  <a-checkbox-group v-model:value="state.checkedList" :options="plainOpitons" />

  <div id="table">
    <!-- <a-table
      :dataSource="a"
      :columns="a"
      size="small"
      :scroll="{ x: 1200, y: 200 }"
    ></a-table> -->
  </div>
</template>
