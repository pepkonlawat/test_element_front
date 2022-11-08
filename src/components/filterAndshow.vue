<script setup lang="ts"></script>
<template>
  <!--checkbox เลือกฝ่าย-->
  <div class="row">
    <div class="column">
      <a-checkbox
        v-model:checked="checkAll"
        :indeterminate="indeterminate"
        @change="onCheckAllChange"
        >ทั้งหมด</a-checkbox
      ><br />

      <a-checkbox>ฝ่ายบริหารทั่วไป</a-checkbox><br />
      <a-checkbox>กลุ่มคอมพิวเตอร์และเครือข่าย</a-checkbox><br />
      <a-checkbox>กลุ่มพัฒนาการบริหารข้อมูล</a-checkbox>
    </div>
    <div class="column">
      <a-checkbox>ผู้บริหาร</a-checkbox><br />
      <a-checkbox>กลุ่มบริหารเทคโนโลยีสารสนเทศและการจัดการ</a-checkbox><br />
      <a-checkbox>กลุ่มพัฒนามาตรฐานและบริการคอมพิวเตอร์</a-checkbox><br />
      <a-checkbox>กลุ่มพัฒนานวัตกรรมดิจิทัล</a-checkbox>
    </div>
  </div>
  <div id="table">
    <!-- <a-table
      :dataSource="a"
      :columns="a"
      size="small"
      :scroll="{ x: 1200, y: 200 }"
    ></a-table> -->
  </div>
</template>
