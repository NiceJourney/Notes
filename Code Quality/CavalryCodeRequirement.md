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