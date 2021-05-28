#### vue -Slot插槽

**子组件中**

```html
<template>
    <div>
        <span class="slot">我是插槽外面的内容</span>
        <slot class="slot">我是插槽预备内容</slot>
    </div>
</template>

<style scoped>
    .slot {
        color: red;
        font-size: 14px;
        font-weight: 400;
    }
</style>
```

**父组件中**

```html
<template>
  <div class="main">
    <slot1>预备插槽内容被插槽内容所替代</slot1>
  </div>

</template>

<script>
import slot1 from '@/views/slotComponents/slot1.vue'
export default {
  name: 'App',
  components: {
    // HelloWorld,
    // Layout,
    // Map
    slot1
  }
}
</script>
```

- **编辑作用域**

  注意：子组件中`slot`标签为一个作用域此时`slot`内部作用域只能在父级作用域编译，之外的作用域为子级作用域，该作用域只能在自己作用域中编译。即，父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

- **后被内容**

  有时为一个插槽设置具体的后备 (也就是默认的) 内容是很有用的，它只会在没有提供内容的时候被渲染。

  ```html
  <button type="submit">
    <slot></slot>
  </button>
  ```

  我们可能希望这个 `<button>` 内绝大多数情况下都渲染文本“Submit”。为了将“Submit”作为后备内容，我们可以将它放在 `<slot>` 标签内：

  ```html
  <button type="submit">
    <slot>Submit</slot>
  </button>
  ```

  现在当我在一个父级组件中使用 `<submit-button>` 并且不提供任何插槽内容时：

  ```html
  <submit-button></submit-button>
  ```

  后备内容“Submit”将会被渲染：

  ```html
  <button type="submit">
    Submit
  </button>
  ```

  若提供则用**提供内容取代后被内容**

element ui时间组件的例子：

```html
<slot
        :visible="pickerVisible"
        :actual-visible="pickerActualVisible"
        :parsed-value="parsedValue"
        :format="format"
        :unlink-panels="unlinkPanels"
        :type="type"
        :default-value="defaultValue"
        @pick="onPick"
        @select-range="setSelectionRange"
        @set-picker-option="onSetPickerOption"
        @mousedown.stop
      ></slot>
```

