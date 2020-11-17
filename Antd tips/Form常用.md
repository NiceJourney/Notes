### 基本配置

```javascript
<Form
	// label 布局
  labelCol={{
    span: 8,
    pull: 1
  }}
  // 右侧选项布局
  wrapperCol={{
    span: 12
  }}
  // form实例, 通过const [form] = Form.useForm()得到
  form={form}
	// 表单名称
  name="paymentRecordModal"
  // 是否显示冒号
  colon={false}
  // 当字段被删除时保留字段值
  preserve={false}
  // 必选提示符号
  requiredMark={false}
  // 类名
  className="typography_sub_heading2 mt-5"
  // 事件
  onFinish={values => handleSubmit(values)}
  onFinishFailed={({ errorFields }) => {
    form.scrollToField(errorFields[0].name);
  }}
></Form>
```



### 校验及报错

基本的校验方式: 

```javascript
// 注意这里返回的是一个Promise
const amountValidator = async (): Promise<void> => {
  const fieldData = form.getFieldValue("name");
  // 判断后, Error中的错误就是要显示在其下方的提示信息
  if (!fieldData) {
    throw new Error("something need to be fixed");
	}
};

<Form.Item
	// name与Form的数据域绑定, 可以通过form.getFieldValue("fieldName")获取, 
  // 也可以通过form.setFieldValue({fieldName: value})
  name={"amountRefunded"}
  // label表示这一表单元素的标题, 类型是React.Node
  label={"Amount Refunded"}
  rules={[
      // 此处一个{}针对一种情况
      // 这里表示1:手动验证及提示消息, 2:正则验证及提示消息, 3:必填及提示消息
      {
        validator: amountValidator // 上面的方法
      },
      {
        pattern: /^\d+$/,
        message: "Please input number."
      },
      {
        required: true,
        message: "Please input refunded amount."
      }
    ]}
>
  <Input prefix={"$"} className="bg-gray-200" />
</Form.Item>
```



- Submit 按钮可以不放在 `<Form.Item></Form.Item>` 标签内, 但是需要放到 `<Form></Form>` 内

