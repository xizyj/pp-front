<template>
  <div class="box">
    <el-form ref="form" :model="form" label-width="80px">
      <el-form-item label="活动名称:">
        <el-input v-model="name"></el-input>
      </el-form-item>
      <el-form-item label="活动名称:">
        <el-input v-model="password"></el-input>
      </el-form-item>
    </el-form>
    <el-form-item>
      <el-button type="primary" @click="login">login</el-button>
      <el-button @click="register">register</el-button>
    </el-form-item>
  </div>
</template>

<script lang="ts">
import { onMounted, ref, reactive, toRefs } from 'vue';
import { get, post } from '@/api/request';
import { ElMessage } from 'element-plus';
export default {
  setup() {
    let path = ref('');
    let keyName = ref('');
    let num = ref(1);
    let page = ref(1);
    let list = ref([]);
    let form = reactive({ name: '', password: '' });
    const changePage = (v: number) => {
      page.value = v;
    };

    const getData = () => {
      get(
        `/search?searchStr=${keyName.value}&filePath=${path.value}&page=${page.value}`
      ).then(res => {
        if (res.code == 0) {
          list.value = res.data.fileList;
        }
      });
    };

    const login = () => {
      let params = {
        name: form.name,
        password: form.password
      };
      post(`/login`, params).then(res => {
        if (res.code == 0) {
          localStorage.setItem('token', res.data);
          ElMessage.success({
            message: '恭喜你，这是一条成功消息',
            type: 'success'
          });
        } else {
          ElMessage.error(res.mesg);
        }
      });
    };

    const register = () => {
      let params = {
        name: form.name,
        password: form.password
      };
      post(`/register`, params).then(res => {
        if (res.code == 0) {
          localStorage.setItem('token', res.data);
          ElMessage.success({
            message: '恭喜你，这是一条成功消息',
            type: 'success'
          });
        } else {
          ElMessage.error(res.mesg);
        }
      });
    };

    onMounted(() => {
      getData();
    });
    return {
      ...toRefs(form),
      login,
      register
    };
  }
};
</script>

<style lang="less">
</style>