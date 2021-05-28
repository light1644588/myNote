#### 时间步骤中涉及表单数据提交参数进行参数格式打包传递的问题解决方案

##### 请求参数

- 将所需要请求的参数放入到vuex里面，在数据请求的时候某一步

页面中from的值通过使用store的方法进行参数传递到vuex要传递的参数里面

##### 回显参数

- 使用vuex内部函数定义好函数将所有的数据进行解构出来，在某一步的页面上使用辅助函数将回显的数据拿出来，显示

###### 总结：

此种请求和回显接口的方式大大提高了对接接口的效率很大程度上解决了对接字段的时间，目前没有什么负面缺陷`(除非vuex性能影响否则此种方法应该是目前来说最好的处理接口的方式)`

​																																					4-15

****



#### 页面数据、请求数据和回显数据处理

- 根据之前的请求数据和回显数据的处理方式，始终将页面上数据放入到vuex中state里面，使用辅助函数(`mapState`、`mapMutation`、`mapGetter`和`mapActions`)将数据从vuex中拿出来显示到页面中，页面中需要的数据均放在页面的data中，比如页面中使用到时间组件，可以将时间空间使用到的数据放入到data中，

  ```javascript
  setTime: {
      actTimeType: 1, 
      dateList: [],
      timeList: [],
  },
  ```

  `vuex`中单个模块的格式

  ```JavaScript
  export default {
      state: { ...},
      mutations: { ...},
      actions: { ...},
      module: { ...}
  }
  ```

  `vuex`主模块

  ```JavaScript
  import Vue from 'vue'
  import Vuex from 'vuex'
  import data from './modules/data'
  import xxx from './modules/user'
  ...
  
  Vue.use(Vuex)
  export default new Vue.Store({
      modules: {
          data,
          xxx,
          ...
  	}
  })
  ```

  ​																																			4-19



