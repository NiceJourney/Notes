1. 学会沟通, 严格遵守开发流程

2. 项目文件夹

   1. 根据项目需求, 把功能模块归类到文件夹

   2. 命名规范: 

      项目大归类文件命名使用名词小写, 比如 common, class;

      组件文件夹和组件文件命名使用PascalCase, 命名不能简写. 命名要具有代表性, 比如:

      组文件夹名: ClassForm, 

      组件名: CreateClass.tsx, ClassForm.tsx

3. script

   1. 一个功能特性文件夹应该包括: 主要的组件文件, 样式文件, 测试文件, index文件. 比如: 

      CheckboxGroup 文件夹

      - CheckboxGroup.tsx
      - CheckboxGroupExtended.tsx
      - CheckboxGroupExtended.test.tsx
      - CheckboxGroup.scss
      - index.ts

   2. vscode安装插件

      1. ESlint
      2. Prettier - Code formatter
      3. Debugger for Chrome
      4. ES7 React/Redux/GraphQL/React-Native snippets
      
   3. 使用 `snippets` 快速搭建代码框架 (typescript)

      1.  `tsrce` ==> 常用类组件框架
      2. `tsrace` ==> 常用函数组件框架

   4. 代码书写规范

      1. [eslint-config-airbnb github](https://github.com/lin-123/javascript)
      2. 函数名使用camelCase，事件处理方法带上前缀 on，或 handle
      3. 类型名, 组件名, 使用PascalCase, graphql: All type names should be in PascalCase(including input types)
      4. Import 放在一起, const, let 声明能放在前面就放在前面
      5. 组件的逻辑最好要在当前组件内完成，不要经常使用props在其他组件来控制逻辑
      6. 不要使用内联样式，(即不要直接在元素上写style， 框架要求除外)
      7. 所有用到的组件在各自文件夹的index文件中export
      8. 变量不要先使用后声明
      9. 不要在同个作用域下声明同名变量
      10. 不要使用未声明的变量
      11. 不要声明了变量却不使用
      12. debugger, alert,  console不要出现在提交的代码里
      13. 数组中不要存在空元素
      14. 不要在循环内部声明函数

4. CSS

   1. 遵循[BEM规范](https://en.bem.info/methodology/quick-start/#introduction), 但是其中的中横线改为下划线，或者命名使用camelCase，blockName__elementName_modifyName, 选择器建议不要嵌套(在scss中如果超过4层应该考虑用嵌套的方式来写)
   2. 如果CSS需要设置为module，则类名的长名称连接只能使用下划线，否则使用camelCase
   3. 设计界面要设置为响应式布局，特别是不要为宽度设置明确的值(框架原因个别除外)，如果框架特性不能很好的设置响应式界面，应该使用普通元素，自己写样式。
   4. 避免CSS表达式： CSS表达式具备求值计算能力，然而每次页面发生重绘时，CSS表达式会影响页面的加载时间。

5. graphQL

   - [中文文档](https://graphql.cn/learn/)	[配合Apollo](https://www.apollographql.com/docs/react/data/queries/)

   1. 查询数据

      项目代码中书写query格式: 

      目前前端有通过 `@graphql-codegen` 库来生成和校验 `schema` , 生成的query和mutation操作名称是PascalCase, 符合官网规范, 并且这个操作名称是唯一的.

      所以前端书写的查询和操作schema操作名称最好使用camelCase, 如 

      `/user.graphql`(书写schema的文件, 后缀为graphql)

      ```js
      query getStudents {
        students {
          _id
          name
          parents {
            _id
            sub
            avatarUrl
            email
            name
            role
          }
        }
      }
      ```

      上面的代码片段表示 ==> query, 操作名称是 getStudents, 查询的后端接口是 students, 需要查询的数据是 _id, name, parents, 其中 parents里面还有其他字段, 需要查询parent对象里面的字段 _id, sub, avatarUrl, email, name, role 等

   2. 善用fragment

      fragment用处就是把一大段重复使用的schema语句放在对象里, 然后通过(...)扩展运算符来使用, 比如上面代码段中的getStudents其中的parents的字段提取出来:

      ```js
      fragment parentField {
        _id
        sub
        avatarUrl
        email
        name
        role
      }
      ```

      

