<!--
 * @Author: your name
 * @Date: 2022-05-13 11:20:13
 * @LastEditors: onepisYa
 * @LastEditTime: 2022-05-15 02:48:41
 * @Description: file content
-->

<script>
import { defineComponent, ref } from 'vue';

export default defineComponent(
  {
    name: 'HelloWorld',
  }
)

</script>

<script setup>
const picked = ref('Tag1')
const checked_names = ref([]);
const selected = ref('');
const vPermissions = {
  mounted(el, binding, vnode)
  {
    vPermissions.updated(el, binding, vnode)
  },
  updated(el, binding, vnode)
  {
    console.log(el, binding, vnode)
    if (binding.value.indexOf(picked.value) === -1)
    {
      el.style.display = 'none'
    } else
    {
      el.style.display = null
    }
  }
}
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
        <div>
          <div v-permissions="['Tag1', 'Tag3']">
            <input
              type="checkbox"
              id="email"
              name="email"
              value="email"
              v-model="checked_names"
            />
            <label for="email">email</label>
            <input
              type="checkbox"
              id="tel"
              name="tel"
              value="tel"
              v-model="checked_names"
            />
            <label for="tel">tel</label>
          </div>
          <div v-permissions="['Tag2', 'Tag3']">
            <button>button</button>
          </div>
          <label for="news" v-permissions="['Tag3']">
            栏目
            <select
              name="select"
              id="select"
              v-model="selected"
            >
              <optgroup label="一组">
                <option value="新闻">新闻</option>
                <option value="游戏">游戏</option>
              </optgroup>
              <optgroup label="二组">
                <option value="经济">经济</option>
                <option value="政治">政治</option>
              </optgroup>
            </select>
          </label>
        </div>


      </fieldset>
    </form>

  </div>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>