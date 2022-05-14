<!--
 * @Author: your name
 * @Date: 2022-05-13 00:38:06
 * @LastEditors: onepisYa pis1@qq.com
 * @LastEditTime: 2022-05-14 23:06:33
 * @Description: file content
-->
<script>
import { defineComponent } from 'vue';
import { ref, watchEffect, reactive, toRefs, defineProps } from 'vue';
export default defineComponent(
  {
    name: 'MySubForm',
  }
)
</script>

<script setup>
class ShowTagFactory
{

  initInstance()
  {
    this.showCheck = false
    this.showButton = false
    this.showSelect = false
  }

  setTrueForAll()
  {
    const properties = Object.getOwnPropertyNames(this)
    properties.forEach(key => this[key] = true)
  }

  constructor (role)
  {
    this.initInstance()
    switch (role)
    {
      case 'Tag1':
        this.showCheck = true
        break
      case 'Tag2':
        this.showButton = true
        break
      case 'Tag3':
        this.setTrueForAll()
        break
      default:
        break
    }
  }
}

const Tags = {
  Tag1: new ShowTagFactory('Tag1'),
  Tag2: new ShowTagFactory('Tag2'),
  Tag3: new ShowTagFactory('Tag3'),
  default: new ShowTagFactory(''),
};

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


function usePicked(props)
{
  let result = reactive({ tag: {} });
  watchEffect(() =>
  {
    if (props.picked !== '')
    {
      result.tag = Tags[props.picked];
    } else
    {
      result.tag = Tags['default'];
    }
  });
  return result;
}

const { tag: t } = toRefs(usePicked(props));
</script>

<template>
  <div>
    <template v-for="(value, key) of t" >
      <div v-show="value">
        {{key}}=> {{value}}<br>
        <!-- 假设 我这里的 key 是 组件名字 那么 下面这样写 是不是就可以比较好的控制显示和隐藏了 
        <component :is="key"></component>
        -->
      </div>
    </template>
   

    {{ t }}
    <div v-show="t.showCheck">
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
    <div v-show="t.showButton">
      <button>button</button>
    </div>
    <label for="news" v-show="t.showSelect">
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
</template>
