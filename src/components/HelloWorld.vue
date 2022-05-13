<script>
import { defineComponent } from 'vue';
import { ref, watchEffect, reactive, toRefs, computed } from 'vue';
import MySubForm from './MySubForm.vue';
export default defineComponent({
  name: 'HelloWorld',
  directives: {
    needpermission: {
      // 指令的定义
      // v-need_permission="ShowCheck"
      created(el) {
        console.log('el', el);
      },

      updated(el, binding, vnode) {
        console.log(el, 'el');
        const require_permission = binding.value;
        const isShow = ref(require_permission);
        console.log(isShow, 'isShow');
        if (!isShow.value) {
          el.parentNode && el.parentNode.removeChild(el);
        } else {
          el.parentNode && el.parentNode.appendChild(el);
        }
      },
    },
  },
});
</script>

<script setup>
class ShowTag {
  showCheck = false;
  showButton = false;
  showSelect = false;
  constructor(opts) {
    Object.assign(this, opts);
  }
}
// 当我想要扩展的时候只需要添加 Tag 的字段并且在 下方添加 对应的应显示的内容即可。
// 需要在 ShowTag 中添加对应的字段
//
const Tags = {
  Tag1: new ShowTag({ showCheck: true }),
  Tag2: new ShowTag({ showButton: true }),
  Tag3: new ShowTag({ showCheck: true, showButton: true, showSelect: true }),
  default: new ShowTag(),
};

const picked = ref(''); // 使用 picked 确定拥有的权限。
const checked_names = ref([]);
const selected = ref('');

function usePicked(picked) {
  let result = reactive({ tag: {} });
  watchEffect(() => {
    if (picked.value !== '') {
      result.tag = Tags[picked.value];
    } else {
      result.tag = Tags['default'];
    }
  });
  return result; // 拥有的权限
}

const { tag: t } = toRefs(usePicked(picked));

/* 
picked 1
显示  checkbox
picked 2 显示 select
picked 3 全部显示 内容 

我想到的方法一是   使用插槽，传递相应的 想要显示的内容即可。 配合 usePicked 使用 一个 数组 去做  v-if

方法二是  给定一个数组 数组里面放着 组件名字 和 插槽名字 ，使用 is 来 动态匹配 组件，template 添加 插槽名字 来匹配 插槽。会产生的问题是 控制显示的颗粒度会变粗，当然如果我将最小颗粒度的 标签封装为对应的组件，也是可以解决的。

方法三 是利用 传参数 特性。 传递一个 props 数组 或者对象 进去。 然后在内部 挂在对应的插槽上面。


另外一种方式  是  使用 指令的方式 
比如 v-p=['one','two', 'three'] 代表 picked  在 这 三个 值的时候都可以显示
v-p＝['one'] 仅仅在 picked ＝ 'one' 的时候显示。
利用权限的概念来处理。 picked 抽象为 拥有的权限 ， 对应元素或者组件上面为 所需要的权限。 当拥有的权限 和 所需要的权限匹配则 显示。否则不显示。 picked 选中的 value 用于 获取 拥有的权限。

然后使用 指令将 所需权限 挂在 对应的元素上， 并且元素上面使用 v-if  来进行计算 所需权限 和 拥有权限是否匹配。

还有一种方式就是 使用 render 函数，但是却没有 使用到  vue 3 的 模板编译优化。

*/
</script>

<template>
  {{ picked }}
  {{ checked_names }}
  {{ selected }}
  {{ t }}
  <form action="">
    <fieldset>
      <legend>onepis</legend>
      <input
        checked
        type="radio"
        name="num"
        value="Tag1"
        id="one"
        v-model="picked"
      />
      <label for="one">一</label>
      <input type="radio" name="num" value="Tag2" id="two" v-model="picked" />
      <label for="two">二</label>
      <input
        checked
        type="radio"
        name="num"
        value="Tag3"
        id="three"
        v-model="picked"
      />
      <label for="three">三</label>
      <hr />

      <MySubForm>
        <!-- <template #one v-if="t.showCheck"> -->
        <template #one>
          <div v-needpermission="t.showCheck">
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
        </template>
        <!-- <template #two v-if="t.showButton"> -->
        <template #two>
        <div>
        <button v-needpermission="t.showButton">button</button>
        </div>
          
        </template>
        <!-- <template #three v-if="t.showSelect"> -->
        <template #three>
          <label for="news" v-needpermission="t.showSelect">
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
        </template>
      </MySubForm>
    </fieldset>
  </form>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>
