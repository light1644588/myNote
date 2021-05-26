### iview 表单验证规则总结

- ###### type:

  `string`: 必须是字符串类型。这是默认类型
  `number`: 必须是数字
  `boolean`: 必须是布尔型的
  `method`: 必须是类型函数
  `regexp`: 必须是 ReGEXP 的实例，或者是在创建新的 ReGEXP 时不会生成异常的字符串
  `integer`: 必须是整数
  `float`: 必须是浮点数
  `array`: 必须是由 Array.isArray 确定的数组
  `object`: 必须是类型对象而不是 Array.isArray
  `enum`: 枚举中必须存在值
  `date`: 按日期确定的值必须有效
  `url`: 必须是 URL 类型
  `hex`: 必须是十六进制
  `email`: 必须是电子邮件类型
  `required`: true | false
  `pattern`: 正则表达式
  `min`: 最小值
  `max`: 最大值
  `Length`: 长度

  { message: "", trigger: '', type: '', enum: ''}
  `message`: 错误信息
  `trigger`: 'change' | 'blur'
  `whiterspace`:

  - 验证数字
    同时进行数字和为空校验：
    校验无效示例：

    ```javascript
    [
      { required: true, message: "排序不能为空", trigger: "blur" },
      { type: "number", message: "请输入排序", trigger: "blur" },
    ];
    ```

    正确示例：

    ```javascript
    const validateSequence = (rule, value, callback) => {
      let regNum = /^.{1,5}$/;
      if (value === "") {
        callback(new Error("输入排序(升序)"));
      } else if (!Number.isInteger(+value)) {
        callback(new Error("输入数字"));
      } else if (!regNum.test(value)) {
        callback(new Error("长度过长"));
      } else {
        callback();
      }
    };

    ruleData: {
      xxx: [
        {
          required: true,
          validator: validateSequence,
          trigger: "blur",
        },
      ];
    }
    ```

  - `ui` 组件库表单验证 `select` 时候验证失败的问题：
    `ui` 组件库自带的表单验证 `select` 时，验证不通过，可能因为组件库的默认校验类型为 `string` | `number` | `array`, 校验时候需要注意数据类型

  - `ui` 组件库表单验证 `DatePicker` 时候验证失败的问题：
    `ui` 组件库自带的表单验证 `DatePicker` 时，校验不通过可能是因为校验类型

  - 多重验证写法

    ```javascript
      ruleValidate: {
        goodsNum: [
          { required: true, message: '数量不能为空', trigger: 'blur' },
          { type: 'string',pattern:/^(([1-9]\d{0,3})|0)(\.\d{0,2})?$/, message:'数量应为正浮点数且不超过9999.99', trigger:'blur'},
        ],
      }
    ```

  - 有时在 `select` 选项需要给个默认选项时，必须在 `data` 中传入 `string` 类型，若是 `number` 类型则无法选中

  - 注意：
    `input` 默认输入为 `string` 类型
    若在表单验证中声明 `type`：`number`，建议 `input` 中加上 `number` 属性，将用户输入自动转换为 `number` 类型

    `select` 单选多选
    提示：单选返回的是一个项，而多选返回的是数组

    `dataPcker` `v-model` 失效
    必须 `on-change` 返回并复制才能实现数据绑定，否则：`value` 无法捕捉日期而选中变动
