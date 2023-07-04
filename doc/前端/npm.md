#### 删除 node_modules
```
npm install rimraf -g 

// 使用命令删除 
rimraf node_modules // 也可以删除其它文件夹或文件
```

#### npm命令
```
npm install -g cnpm --registry=https://registry.npm.taobao.org 

npm config set registry https//registry.npm.taobao.org // 换源

npm config set prefix "D:\programme\nodeJS_resource\node_global" // 修改全局
npm config set cache "D:\programme\nodeJS_resource\node_cache" // 修改缓存

npm ls --depth 0 -g // 看看哪些失效了

npm prune -g // 修剪下全局包

npm rebuild -g // 重建下全局包

npm update -g // 更新下全局包的版本

npm cache clear --force -g // 删除全局包的缓存（慎重）
```
