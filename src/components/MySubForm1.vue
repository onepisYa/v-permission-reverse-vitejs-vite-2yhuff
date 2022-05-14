<!--
 * @Author: your name
 * @Date: 2022-05-13 00:38:06
 * @LastEditors: onepisYa pis1@qq.com
 * @LastEditTime: 2022-05-14 23:50:34
 * @Description: file content
-->
<script>
import { ref, defineProps, defineComponent} from 'vue';
export default defineComponent(
  {
    name: 'MySubForm',
  }
)
</script>

<script setup>
// local directives 指令
const vPermissions = {
  mounted(el, binding, vnode) {
    vPermissions.updated(el, binding, vnode)
  },
  updated(el, binding, vnode) {
    console.log(el, binding, vnode)
    console.log(props);
    if (binding.value.indexOf(props.picked) === -1) {
      el.style.display = 'none'
    }else{
      el.style.display = null
    }
  }
}

const checked_names = ref([]);
const selected = ref('');
const props = defineProps(
  {
    picked: {
      type: String,
      required: true
    }
  }
)
</script>

<template>
  <div>
    <div  v-permissions="['Tag1', 'Tag3']">
      <input type="checkbox" id="email" name="email" value="email" v-model="checked_names" />
      <label for="email">email</label>
      <input type="checkbox" id="tel" name="tel" value="tel" v-model="checked_names" />
      <label for="tel">tel</label>
    </div>
    <div v-permissions="['Tag2', 'Tag3']">
      <button>button</button>
    </div>
    <label for="news" v-permissions="['Tag3']">
      栏目
      <select name="select" id="select" v-model="selected">
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
</template>
