<!--
 * @Author: your name
 * @Date: 2022-05-13 11:20:13
 * @LastEditors: onepisYa pis1@qq.com
 * @LastEditTime: 2022-05-14 23:50:58
 * @Description: file content
-->

<script>
import { defineComponent } from 'vue';
import { ref, } from 'vue';
import MySubForm from './MySubForm1.vue';

export default defineComponent(
  {
    name: 'HelloWorld',
  }
)

</script>

<script setup>
const picked = ref('Tag1')
</script>

<template>
  <div>
    <form action="">
      <fieldset>
        <legend>onepis</legend>
        <template v-for="(item, idx) in ['one', 'two', 'three']">
          <input
            type="radio"
            name="num"
            :value='`Tag${idx + 1}`'
            :id='item'
            v-model='picked'
          />
          <label :for="item">{{ item }}</label>
        </template>
        <hr />
        <MySubForm :picked="picked">
        </MySubForm>
      </fieldset>
    </form>

  </div>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>