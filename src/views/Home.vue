<template>
  <div class="home">
    <div class="list">
      <div class="item" v-for="item in list" :key="item">
        <img :src="item.src" />
      </div>
    </div>
    <div class="bottom">
      <el-button @click="getData">button</el-button>
    </div>
  </div>
</template>

<script lang="ts">
import { onMounted, ref, reactive, toRefs } from 'vue';
import { get, post } from '@/api/request';
export default {
  setup() {
    let list = ref([]);
    const getData = () => {
      get(`/buy`).then(res => {
        if (res.code == 0) {
          res.data.list.forEach(item => {
            list.value.push(item);
          });
        }
      });
    };
    return {
      getData,
      list
    };
  }
};
</script>
