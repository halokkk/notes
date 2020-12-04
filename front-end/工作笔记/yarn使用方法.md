### yarn使用方法

> [官方文档]: https://yarn.bootcss.com/docs/usage/

###### **初始化一个新项目**

```
yarn init
```

###### **添加依赖包**

```
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

###### **将依赖项添加到不同依赖项类别中**

分别添加到 `devDependencies`、`peerDependencies` 和 `optionalDependencies` 类别中：

> [yarn依赖的类型]: https://yarn.bootcss.com/docs/dependency-types/

```
yarn add [package] --dev
yarn add [package] --peer
yarn add [package] --optional
```

###### **升级依赖包**

```
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

###### **移除依赖包**

```
yarn remove [package]
```

###### **安装项目的全部依赖**

```
yarn
```

或者

```
yarn install
```

