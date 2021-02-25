# Vue版本Echarts实现中国地图三级钻取
# 依据此功能 实现公司大屏展示项目 实现地图切换的城市散点图数据展示
# 根据echarts的不同的配置来实现
# 大屏项目难点 熟悉echarts地图的配置 渲染地图前必须要加载对应城市的json数据 重要的四部过程
# 	let cityJson = await this.loadCityJson(item.areaCode);
#    let option = await this.cityCodeGetOption(item.areaCode, item.cityName, cityJson,"bottom");
#    echarts.registerMap(item.cityName, cityJson);    关键需要注册
#    echarts.init(document.getElementById(`rank-${index + 1}`)).setOption(option, true);
## 1、演示地址
[http://gh.dongkelun.com/vue-echarts-map/#/](http://gh.dongkelun.com/vue-echarts-map/#/)

## 2、运行及部署

### 2.1 本地运行
安装依赖

```
npm install
```
运行
```
npm run dev
```
访问：[localhost:8080](localhost:8080)
### 2.2 部署
打包
```
npm run build
```
部署到tomcat参考：[通过Vue CLI 快速创建Vue项目并部署到tomcat](https://dongkelun.com/2018/11/19/vueCliCreateProject/)


## 3、博客地址
[https://dongkelun.com/2019/02/18/vueEchartsMap/](https://dongkelun.com/2019/02/18/vueEchartsMap/)

## 4、html+css+js版本地址
[https://github.com/dongkelun/echarts-map](https://github.com/dongkelun/echarts-map)


