<template>
	<div class="app-container" id="new-screen">
		<div class="full-screen-btn" @click="handleFullScreen"></div>
		<div id="time">{{ time }}</div>
		<div class="left-col">
			<div class="bg top">
				<div class="title-name">设备占比及数量</div>
				<div class="content" id="left-top-1"></div>
				<div class="text" style="position: absolute;top: 50%;left: 23%;font-size: 22px;color: #7491D6;">占比</div>
				<div class="right-list">
					<div class="right-item">
						<div class="right-title">
							<div class="rect-icon" style="background: #18daff"></div>
							<div class="item-name">两轮车</div>
						</div>
						<div class="num">{{ car2Num }}</div>
					</div>
					<div class="right-item">
						<div class="right-title">
							<div class="rect-icon" style="background: #3185ff"></div>
							<div class="item-name">三轮车</div>
						</div>
						<div class="num">{{ car3Num }}</div>
					</div>
					<div class="right-item">
						<div class="right-title">
							<div class="rect-icon" style="background: #1be08a"></div>
							<div class="item-name">60v电池</div>
						</div>
						<div class="num">{{ bat60Num }}</div>
					</div>
					<div class="right-item">
						<div class="right-title">
							<div class="rect-icon" style="background: #b187fe"></div>
							<div class="item-name">48v电池</div>
						</div>
						<div class="num">{{ bat48Num }}</div>
					</div>
					<div class="right-item">
						<div class="right-title">
							<div class="rect-icon" style="background: #ffb543"></div>
							<div class="item-name">换电柜</div>
						</div>
						<div class="num">{{ cabNum }}</div>
					</div>
				</div>
			</div>
			<div class="bg center">
				<p class="title-name">减少碳排放量</p>
				<div class="content" style="padding-bottom: 2vw">
					<div class="left-content">
						<div class="bg-img" style="color: #bbecff">
							<div class="num">{{ discharge }}</div>
						</div>
						<div>减少碳排放 (吨)</div>
					</div>
					<div class="right-content">
						<div class="bg-img" style="color: #1be08a">
							<div class="num">{{ treeNum }}</div>
						</div>
						<div>相当于植树 (棵)</div>
					</div>
				</div>
			</div>
			<div class="bg bottom" style="padding: 2vw 0vw 1vw 1vw;width: 110%;">
				<div class="line">
					<img src="./img/bottom-1.png" alt="" />
					<div class="center">
						<p style="color: #bbecff">{{ TTodo }}</p>
						<p>车-累计里程 (km)</p>
					</div>
					<div class="right">
						<p style="color: #bbecff">{{ TDodo }}</p>
						<p>今日累计里程 (km)</p>
					</div>
				</div>
				<div class="line" style="margin-top: 1.5vw">
					<img src="./img/bottom-2.png" alt="" />
					<div class="center">
						<p style="color: #02ff91">{{ TTCbat }}</p>
						<p>电-累计换电 (次)</p>
					</div>
					<div class="right">
						<p style="color: #02ff91">{{ TDCbat }}</p>
						<p>今日累计换电 (次)</p>
					</div>
				</div>
			</div>
		</div>
		<div class="center-col">
			<div class="num-list-show">
				<div class="num-itme">
					<div class="title">总用户数</div>
					<div class="num">{{ userNum }}</div>
				</div>
				<div class="num-itme">
					<div class="title">总设备数</div>
					<div class="num">{{ vNum }}</div>
				</div>
				<div class="num-itme">
					<div class="title">总订单数</div>
					<div class="num">{{ orderNum }}</div>
				</div>
			</div>
			<div class="city-select">
				<el-select v-model="selValue" placeholder="请选择" popper-class="selectFrom" :popper-append-to-body="false" @change="handleSelCity($event)">
					<el-option v-for="item in cityList" :key="item.cityName" :label="item.cityName" :value="item.cityName"> </el-option>
				</el-select>
			</div>
			<div class="bottom-city-rank">
				<div class="city-list" @click="bottomMapClick($event)">
					<div class="city-item">
						<div id="rank-1" style="width: 80%; height: 70%"></div>
						<div class="click-mask">{{ thirdRank[0] ? thirdRank[0].cityName : "" }}</div>
					</div>
					<div class="city-item">
						<div id="rank-2" style="width: 80%; height: 70%"></div>
						<div class="click-mask">{{ thirdRank[1] ? thirdRank[1].cityName : "" }}</div>
					</div>
					<div class="city-item">
						<div id="rank-3" style="width: 80%; height: 70%"></div>
						<div class="click-mask">{{ thirdRank[2] ? thirdRank[2].cityName : "" }}</div>
					</div>
				</div>
			</div>
			<div id="map" style="width: 100%; height: 100%"></div>
			<div class="bottom-bg" :class="[selValue.length > 4 ? 'add-length' : '']">{{ selValue }}车电柜分布大数据</div>
		</div>
		<div class="right-col">
			<div class="bg top">
				<div class="title-name">24h换电监控</div>
				<div class="content" id="Chart3" style="height: 90%; padding: 1vw 1.8vw 1.2vw 2.8vw"></div>
			</div>
			<div class="bg center right-2">
				<p class="title-name">30天内车电柜离线预警</p>
				<div class="content" id="Chart4" style="padding: 1vw 1.8vw 1.2vw 2.8vw"></div>
			</div>
			<div class="bg bottom right-bg">
				<p class="title-name">top5大客户占比</p>
				<div class="content" style="display: flex; align-items: center; justify-content: center">
					<div class="ratio-list">
						<div class="ratio-item" v-for="(item, index) in userData" :key="index">
							<div>{{ item.childrenModuleName }}</div>
							<el-progress :percentage="item.percentage" :color="item.color" style="width: 75%"></el-progress>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import Api from "@/api/claims";
import tyle from "./json/map.js";
import "../../../node_modules/echarts/map/js/china.js";
import echarts from "echarts";
import axios from "axios";
import cityMap from "./china-main-city-map.js";
function initMapData(mapJson) {
	var mapData = [];
	for (var i = 0; i < mapJson.features.length; i++) {
		mapData.push({
			name: mapJson.features[i].properties.name,
			//id:mapJson.features[i].id
		});
	}
	return mapData;
}
export default {
	components: {},
	data() {
		return {
			title: "",
			time: "",
			memo: "",
			jobs: "",
			orderNum: "",
			equipment: "",
			companies: "",
			warning: "",
			userNum: null,
			vNum: null,
			orderNum: null,
			bat48Num: null,
			bat60Num: null,
			cabNum: null,
			car3Num: null,
			car2Num: null,
			TDCbat: null,
			TTCbat: null,
			TDodo: null,
			TTodo: null,
			todat_cebinet: null,
			today_battery: null,
			treeNum: null,
			discharge: null,
			circle: 0,
			circle1: 0,
			proNum: null,
			ratioTotal: 0,
			userData: [],
			cityList: [],
			selValue: "上海市",
			provinceData: [], // 已有业务省数据
			loadMapParam: {
				option: {},
				dom: null,
			},
			firstRank: [],
			secRank: [],
			thirdRank: [],
			detailData: {},
		};
	},
	methods: {
		// 城市选择下拉
		async handleSelCity(e) {
			console.log("e====>", e);
			let cityCode = cityMap[e];
			let cityJson = await this.loadCityJson(cityCode);
			let chartOption = {};
			if(e == '全国'){
				let data = await this.getChinaData();
				chartOption = await this.getChinaOption(data);
			}else{
				chartOption = await this.cityCodeGetOption(cityCode, e, cityJson);
			}
			echarts.registerMap(e, cityJson);
			this.loadCityMap(chartOption);
		},
		// 设置下拉框城市数据
		async setSelCity() {
			let res = await this.getSelData(1000);
			if (res) {
				res.items.unshift({
					areaCode: "100000",
					cityId: 999999,
					cityName: "全国",
				});
				this.cityList = res.items;
			}
		},
		// 获取底部前3名数据
		async initBottomRankOpt() {
			let res = await this.getSelData(3);
			if (res && res.items.length > 0) {
				res.items.forEach(async (item, index) => {
					let cityJson = await this.loadCityJson(item.areaCode);
					let option = await this.cityCodeGetOption(item.areaCode, item.cityName, cityJson,"bottom");
					echarts.registerMap(item.cityName, cityJson);
					echarts.init(document.getElementById(`rank-${index + 1}`)).setOption(option, true);
				});
				this.thirdRank = res.items;
			}
		},
		// 底部3个地图点击
		async bottomMapClick(e) {
			this.selValue = e.target.innerText;
			this.handleSelCity(e.target.innerText);
		},
		// 获取下拉框数据
		getSelData(rank) {
			return new Promise((resolve) => {
				Api.bigScreenGetTopProvince(rank).then((res) => {
					resolve(res);
				});
			});
		},
		// 加载city的json文件
		loadCityJson(areaCode) {
			let cityJson = require("./json/map/" + areaCode + ".json");
			return cityJson;
		},
		// 加载地图
		loadCityMap(option, dom) {
			// 初始化echarts
			let mapDom = dom ? dom : document.getElementById("map");
			let map = echarts.init(mapDom);
			map.setOption(option);
		},
		// 获取全国echart option
		getChinaOption(data) {
			let option = {
				geo: {
					show: true,
					// map: 'china' | '四川'
					// 必须要先引入了对应地图的js文件或者json文件，在这一步的时候，echarts会自动将对应的JS文件注入，地图才会显示.
					map: "china",
					label: {
						normal: {
							show: false,
						},
						emphasis: {
							show: false,
						},
					},
					itemStyle: {
						// 定义样式
						normal: {
							// 普通状态下的样式
							// areaColor: "#3A4F7A",
							// borderColor: "#B3D5FB",
							// borderWidth: 1, //设置外层边框
							// shadowColor: "rgba(179,213,251, 0.6)",
							// shadowBlur: 1,
							// color: "#fff",
							areaColor: '#00FFFF',
							borderWidth: 2,
							borderColor: '#2AB8FF',
							shadowColor: '#2AB8FF',
							shadowBlur: 10,
							shadowOffsetX: 1,
							shadowOffsetY: 2
						},
						emphasis: {
							// 高亮状态下的样式
							areaColor: "#fff",
							color: "#fff",
						},
					},
					roam: false, //地图设置不可拖拽，固定的
				},
				selectedMode: "single",
				series: [
					{
						type: "map",
						map: "china",
						geoIndex: 1,
						aspectScale: 0.75, //长宽比
						showLegendSymbol: false, // 存在legend时显示
						tooltip: {
							show: false,
						},
						label: {
							normal: {
								show: false,
							},
							emphasis: {
								show: true,
								textStyle: {
									color: "#fff",
								},
							},
						},
						roam: false,
						itemStyle: {
							normal: {
								areaColor: "#19396F",
								borderColor: "#B2D4FA",
								borderWidth: 1,
								shadowColor: "rgba(179,213,251, 0.6)",
								shadowBlur: 5,
								color: "#fff",
							},
							emphasis: {
								show: true,
								areaColor: "#30508B",
							},
						},
					},
					{
						name: "",
						type: "effectScatter",
						coordinateSystem: "geo",
						data: data,
						symbolSize: function (val) {
							if (val[2] < 100) {
								return 5;
							} else if (val[2] < 300 && val[2] > 100) {
								return 10;
							} else if (val[2] > 300 && val[2] < 500) {
								return 10;
							} else if (val[2] > 500 && val[2] < 600) {
								return 20;
							} else if (val[2] > 600 && val[2] < 1000) {
								return 20;
							} else if (val[2] > 1000 && val[2] < 1200) {
								return 30;
							} else if (val[2] > 1200 && val[2] < 1500) {
								return 40;
							} else {
								return 40;
							}
						},
						showEffectOn: "render",
						rippleEffect: {
							brushType: "stroke",
						},
						hoverAnimation: true,
						label: {
							normal: {
								formatter: "{b}",
								position: "right",
								show: true,
							},
						},
						itemStyle: {
							normal: {
								color: "#ffb543",
								shadowBlur: 1,
								shadowColor: "#0F6279",
							},
						},
						zlevel: 1,
					},
				],
			};
			return option;
		},
		// 获取城市echart option
		getCityMapOption(name, data, mapJson) {
			// console.log("传入name", name);
			// console.log("传入data", data);
			// console.log("传入mapJson", mapJson);
			let option = {
				tooltip: {
					trigger: "item",
					show: false,
				},
				geo: {
					// map: 'china' | '四川'
					// 必须要先引入了对应地图的js文件或者json文件，在这一步的时候，echarts会自动将对应的JS文件注入，地图才会显示.
					map: name,
					geoIndex: 2,
					itemStyle: {
						// 定义样式
						normal: {
							// 普通状态下的样式
							areaColor: "#3A4F7A",
							borderColor: "#B3D5FB",
							borderWidth: 5, //设置外层边框
							shadowColor: "rgba(179,213,251, 0.3)",
							shadowBlur: 30,
							color: "#fff",
						},
						emphasis: {
							show: true,
							color: "#fff",
							areaColor: "#30508B",
							textStyle: {
								color: "#fff",
							},
						},
					},
				},
				series: [
					{
						type: "map",
						map: name,
						data: initMapData(mapJson),
						aspectScale: 0.75, //长宽比
						emphasis: {
							show: true,
							textStyle: {
								color: "#fff",
							},
						},
						label: {
							emphasis: {
								show: true,  //开启悬浮事件
								color: '#fff'//字体颜色
							}
						},
						itemStyle: {
							normal: {
								areaColor: "#0e2d61",
								borderColor: "#4b6a99",
								borderWidth: 1,
								color: "#fff",
							},
							emphasis: {
								areaColor: "#30508B",
								color: "#fff",
								show: true,
								textStyle: {
									color: "#fff",
								},
							},
						},
					},
					{
						name: "",
						type: "effectScatter",
						coordinateSystem: "geo",
						data: data,
						tooltip: {
							show: true,
							formatter: function (params) {
								// 显示站点
								return params.data.value[3];
							},
						},
						symbolSize: function (val) {
							if (val[2] == 1) {
								return 10;
							} else if (val[2] == 2) {
								return 15;
							} else {
								return 20;
							}
						},
						showEffectOn: "render",
						rippleEffect: {
							brushType: "stroke",
							period:10,
						},
						label: {
							normal: {
								formatter: "{b}",
								position: "right",
								show: false,
							},
						},
						itemStyle: {
							normal: {
								color: "#ffb543",
								shadowBlur: 1,
								shadowColor: "#0F6279",
							},
						},
						hoverAnimation: true,
						zlevel: 1,
					},
				],
			};
			return option;
		},
		// 获取底部echart option
		getBottomOpt(param, data) {
			let option = {
				geo: {
					// map: 'china' | '四川'
					// 必须要先引入了对应地图的js文件或者json文件，在这一步的时候，echarts会自动将对应的JS文件注入，地图才会显示.
					map: param,
					itemStyle: {
						// 定义样式
						normal: {
							// 普通状态下的样式
							areaColor: "#0e2d61",
							borderColor: "#B3D5FB",
							borderWidth: 4, //设置外层边框
							// shadowColor: "rgba(179,213,251, 0.2)",
							// shadowBlur: 30,
							// shadowOffsetY:10,
						},
					},
				},
				series: [
					{
						type: "map",
						map: param,
						geoIndex: 1,
						aspectScale: 0.75, //长宽比
						showLegendSymbol: true, // 存在legend时显示
						tooltip: {
							show: true,
						},
						label: {
							normal: {
								show: false,
							},
							emphasis: {
								show: true,
								textStyle: {
									color: "#fff",
								},
							},
						},
						roam: false,
						itemStyle: {
							normal: {
								areaColor: "#263961",
								borderColor: "#263961",
								borderWidth: 1,
							},
						},
					},
					{
						name: "",
						type: "effectScatter",
						coordinateSystem: "geo",
						data: data,
						tooltip: {
							formatter: "{b}",
						},
						symbolSize: 2,
						showEffectOn: "render",
						hoverAnimation: false,
						label: {
							normal: {
								formatter: "{b}",
								position: "right",
								show: false,
							},
						},
						itemStyle: {
							normal: {
								color: "#ffb543",
							},
						},
						zlevel: 1,
					},
				],
			};
			return option;
		},
		// 根据城市名称得到option
		cityCodeGetOption(cityCode, cityName, mapJson, type) {
			let option = {};
			return new Promise((resolve) => {
				if (!cityCode) return;
				let rank = 1;
				if(this.cityList && this.cityList.length > 0){
					this.cityList.forEach((item,index) => {
						if(item.areaCode == cityCode){
							rank = index;
						}
					})
				}
				Api.bigScreenGetSiteDistribution(cityCode,rank).then((res) => {
					if (res) {
						let data = res.items.map((item) => {
							return {
								// name: item.siteName,
								value: [
									item.longitude,
									item.latitude,
									item.randomSize, //数量
									item.siteName, // 站点名称
								],
							};
						});
						option = type == "bottom" ? this.getBottomOpt(cityName, data) : this.getCityMapOption(cityName, data, mapJson);
						resolve(option);
					}
				});
			});
		},
		// 获取城市排名数据
		bigScreenGetTopProvince(rank) {
			return new Promise((resolve) => {
				Api.bigScreenGetTopProvince(rank).then((res) => {
					resolve(res);
				});
			});
		},
		echart1() {
			Api.getModuleData({ moduleName: "business" })
				.then((res) => {
					if (res.success) {
						this.businessData = res.items;
						for (var i in this.businessData) {
							if (this.businessData[i].childrenModuleName == "user_total_num") {
								this.userNum = this.thousandNum(this.businessData[i].showData01);
							} else if (this.businessData[i].childrenModuleName == "device_total_num") {
								this.vNum = this.thousandNum(this.businessData[i].showData01);
							} else if (this.businessData[i].childrenModuleName == "order_total_num") {
								this.orderNum = this.thousandNum(this.businessData[i].showData01);
							}
						}
					} else {
						this.$message.error(res.message);
					}
				})
				.catch(() => {});
		},
		echart2() {
			Api.getModuleData({ moduleName: "device_view" })
				.then((res) => {
					if (res.success) {
						this.devData = res.items;
						for (var i in this.devData) {
							if (this.devData[i].childrenModuleName == "battery_48_num") {
								this.bat48Num = this.devData[i].showData01;
							} else if (this.devData[i].childrenModuleName == "battery_60_num") {
								this.bat60Num = this.devData[i].showData01;
							} else if (this.devData[i].childrenModuleName == "cabinet_num") {
								this.cabNum = this.devData[i].showData01;
							} else if (this.devData[i].childrenModuleName == "three_car_num") {
								this.car3Num = this.devData[i].showData01;
							} else if (this.devData[i].childrenModuleName == "two_car_num") {
								this.car2Num = this.devData[i].showData01;
							}
						}
						var _this = this;
						var placeHolderStyle = {
							normal: {
								label: {
									show: false,
								},
								labelLine: {
									show: false,
								},
								color: "rgba(0, 0, 0, 0)",
								borderColor: "rgba(0, 0, 0, 0)",
								borderWidth: 0,
							},
						};
						var myChart = this.$echarts.init(document.getElementById("left-top-1"));
						myChart.setOption({
							tooltip: {
								trigger: "item",
							},
							color: ["#18DAFF", "#3185FF", "#1BE08A", "#B187FE", "#FFB543"],
							series: [
								{
									name: "",
									type: "pie",
									clockWise: true,
									// aspectScale: 0.75,
									radius: ["50%", "60%"],
									center: ["28%", "46%"],
									hoverAnimation: true,
									label: {
										normal: {
											show: true,
											position: "center",
										},
									},
									labelLine: {
										normal: {
											show: true,
										},
									},
									data: [
										{
											value: this.car2Num,
											// name: "两轮车",
											itemStyle: {
												normal: {
													borderWidth: 0,

													borderColor: "#18DAFF",
													shadowColor: "#18DAFF",
												},
											},
										},
										{
											value: this.car3Num,
											// name: "三轮车",
											itemStyle: {
												normal: {
													borderWidth: 0,

													borderColor: "#3185FF",
													shadowColor: "#3185FF",
												},
											},
										},
										{
											value: this.bat60Num,
											// name: "60v电池",
											itemStyle: {
												normal: {
													borderWidth: 0,

													borderColor: "#1BE08A",
													shadowColor: "#1BE08A",
												},
											},
										},
										{
											value: this.bat48Num,
											// name: "48v电池",
											itemStyle: {
												normal: {
													borderWidth: 0,

													borderColor: "#B187FE",
													shadowColor: "#B187FE",
												},
											},
										},
										{
											value: this.cabNum,
											// name: "换电柜",
											itemStyle: {
												normal: {
													borderWidth: 0,

													borderColor: "#FFB543",
													shadowColor: "#FFB543",
												},
											},
										},
									],
								},
							],
						});
					} else {
						this.$message.error(res.message);
					}
				})
				.catch(() => {});
		},
		echart3() {
			var myColor = ["#3185FF", "#1BE08A", "#B187FE", "#FFB543", "#18DAFF"];
			var valdata = [];
			Api.getModuleData({ moduleName: "user_view" })
				.then((res) => {
					if (res.success) {
						this.userData = res.items;
						let total = 0;
						for (let i in res.items) {
							total += parseFloat(res.items[i].showData01);
						}
						for (var i in res.items) {
							this.userData[i].childrenModuleName = res.items[i].childrenModuleName;
							this.userData[i].showData01 = res.items[i].showData01;
							this.userData[i].percentage = parseInt((parseFloat(res.items[i].showData01) / total) * 100);
							// this.userData[i].percentage = 50;s
						}
						for (let i in myColor) {
							this.userData[i].color = myColor[i];
						}
					} else {
						this.$message.error(res.message);
					}
				})
				.catch(() => {});
		},
		echartpatch() {
			Api.getModuleData({ moduleName: "carbon_view" })
				.then((res) => {
					if (res.success) {
						this.odoData = res.items;
						for (var i in this.odoData) {
							if (this.odoData[i].childrenModuleName == "total_carbon_discharge") {
								this.discharge = this.thousandNum(this.odoData[i].showData01);
							} else if (this.odoData[i].childrenModuleName == "total_tree_num") {
								this.treeNum = this.thousandNum(this.odoData[i].showData01);
							}
						}
					} else {
						this.$message.error(res.message);
					}
				})
				.catch(() => {});
		},
		echart4() {
			Api.getModuleData({ moduleName: "odometer_view" })
				.then((res) => {
					if (res.success) {
						this.odoData = res.items;
						for (var i in this.odoData) {
							if (this.odoData[i].childrenModuleName == "today_change_battery") {
								this.TDCbat = this.thousandNum(this.odoData[i].showData01);
							} else if (this.odoData[i].childrenModuleName == "today_odometer") {
								this.TDodo = this.thousandNum(this.odoData[i].showData01);
							} else if (this.odoData[i].childrenModuleName == "total_change_battery") {
								this.TTCbat = this.thousandNum(this.odoData[i].showData01);
							} else if (this.odoData[i].childrenModuleName == "total_odometer") {
								this.TTodo = this.thousandNum(this.odoData[i].showData01);
							}
						}
					} else {
						this.$message.error(res.message);
					}
				})
				.catch(() => {});
		},
		echart5() {
			var x1 = [];
			var lowy1 = [];
			var cany1 = [];
			var ordery1 = [];
			var x2 = [];
			var lowy2 = [];
			var cany2 = [];
			var ordery2 = [];
			var x3 = [];
			var lowy3 = [];
			var cany3 = [];
			var ordery3 = [];
			var x4 = [];
			var lowy4 = [];
			var cany4 = [];
			var ordery4 = [];
			Api.getModuleData({ moduleName: "lowPowerAndChangeBattery" })
				.then((res) => {
					if (res.success) {
						this.warningBat = res.items;
						for (var j in this.warningBat) {
							if (this.warningBat[j].childrenModuleName == "changeBattery") {
								for (var i in this.warningBat[j].list[0].list) {
									cany1.push(this.warningBat[j].list[0].list[i].showData01);
								}
								for (var i in this.warningBat[j].list[1].list) {
									cany2.push(this.warningBat[j].list[1].list[i].showData01);
								}
								for (var i in this.warningBat[j].list[2].list) {
									cany3.push(this.warningBat[j].list[2].list[i].showData01);
								}
								for (var i in this.warningBat[j].list[3].list) {
									cany4.push(this.warningBat[j].list[3].list[i].showData01);
								}
							}
							if (this.warningBat[j].childrenModuleName == "changeBatteryOrderNum") {
								for (var i in this.warningBat[j].list[0].list) {
									ordery1.push(this.warningBat[j].list[0].list[i].showData01);
								}
								for (var i in this.warningBat[j].list[1].list) {
									ordery2.push(this.warningBat[j].list[1].list[i].showData01);
								}
								for (var i in this.warningBat[j].list[2].list) {
									ordery3.push(this.warningBat[j].list[2].list[i].showData01);
								}
								for (var i in this.warningBat[j].list[3].list) {
									ordery4.push(this.warningBat[j].list[3].list[i].showData01);
								}
							}
							if (this.warningBat[j].childrenModuleName == "lowPower") {
								for (var i in this.warningBat[j].list[0].list) {
									lowy1.push(this.warningBat[j].list[0].list[i].showData01);
								}
								for (var i in this.warningBat[j].list[1].list) {
									lowy2.push(this.warningBat[j].list[1].list[i].showData01);
								}
								for (var i in this.warningBat[j].list[2].list) {
									lowy3.push(this.warningBat[j].list[2].list[i].showData01);
								}
								for (var i in this.warningBat[j].list[3].list) {
									lowy4.push(this.warningBat[j].list[3].list[i].showData01);
								}
							}
						}
						for (var i in this.warningBat[0].list[0].list) {
							x1.push(this.warningBat[0].list[0].list[i].childrenModuleName);
						}
						for (var i in this.warningBat[0].list[1].list) {
							x2.push(this.warningBat[0].list[1].list[i].childrenModuleName);
						}
						for (var i in this.warningBat[0].list[2].list) {
							x3.push(this.warningBat[0].list[2].list[i].childrenModuleName);
						}
						for (var i in this.warningBat[0].list[3].list) {
							x4.push(this.warningBat[0].list[3].list[i].childrenModuleName);
						}
						this.lowWarning(x1, lowy1, cany1, ordery1);
						setInterval(() => {
							this.circle++;
							if (this.circle == 5) {
								this.circle = 1;
							}
							if (this.circle == 1) {
								this.lowWarning(x4, lowy4, cany4, ordery4);
							} else if (this.circle == 2) {
								this.lowWarning(x3, lowy3, cany3, ordery3);
							} else if (this.circle == 3) {
								this.lowWarning(x2, lowy2, cany2, ordery2);
							} else if (this.circle == 4) {
								this.lowWarning(x1, lowy1, cany1, ordery1);
							}
						}, 8000);
					} else {
						this.$message.error(res.message);
					}
				})
				.catch(() => {});
		},
		lowWarning(x, lowy, cany, ordery) {
			var myChart3 = this.$echarts.init(document.getElementById("Chart3"));
			myChart3.clear();
			myChart3.setOption({
				grid: {
					top: "22%",
					left: "0%",
					right: "5%",
					bottom: "8%",
					containLabel: true,
				},
				tooltip: {
					trigger: "axis",
					axisPointer: {
						type: "cross",
					},
				},
				legend: {
					x: "center",
					data: [{ name: "低电量", icon: "rect" }, { name: "可换电池", icon: "rect" }, { name: "换电订单" }],
					textStyle: {
						fontSize: "18",
						color: "#b9babf",
					},
				},
				xAxis: [
					{
						type: "category",
						data: x,
						axisPointer: {
							type: "shadow",
						},
						axisLabel: {
							show: true,
							textStyle: {
								color: "#7491D6", //更改坐标轴文字颜色
								fontSize: 14, //更改坐标轴文字大小
							},
						},
					},
				],
				yAxis: [
					{
						type: "value",
						name: "",
						axisLabel: {
							show: false,
						},
						axisTick: {
							show: false,
						},
						axisLine: {
							lineStyle: {
								color: "#658CE6",
								opacity: 0,
							},
						},
						splitLine: {
							lineStyle: {
								color: "#658CE6",
								opacity: 0.4,
							},
						},
						axisLabel: {
							show: true,
							textStyle: {
								color: "#7491D6", //更改坐标轴文字颜色
								fontSize: 16, //更改坐标轴文字大小
							},
						},
					},
				],
				series: [
					{
						name: "低电量",
						type: "bar",
						data: lowy,
						itemStyle: {
							normal: {
								label: {
									show: false,
									position: "top",
									textStyle: {
										color: "#7491D6",
										fontSize: 10,
									},
								},
								color: new echarts.graphic.LinearGradient(0, 1, 0, 0, [
									{ offset: 0, color: "#AF7612" },
									{ offset: 1, color: "#FEC733" },
								]),
							},
						},
					},
					{
						name: "可换电池",
						type: "bar",
						data: cany,
						itemStyle: {
							normal: {
								label: {
									show: false,
									position: "top",
									textStyle: {
										color: "#7491D6",
										fontSize: 10,
									},
								},
								color: new echarts.graphic.LinearGradient(0, 1, 0, 0, [
									{ offset: 0, color: "#379BD0" },
									{ offset: 1, color: "#18DAFF" },
								]),
							},
						},
					},
					{
						name: "换电订单",
						type: "line",
						smooth: true,
						data: ordery,
						itemStyle: {
							normal: {
								color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [
									{ offset: 0, color: "#3185FF" },
									{ offset: 1, color: "#3185FF" },
								]),
							},
						},
						// 折线下部阴影
						areaStyle: {
							normal: {
								color: {
									x: 0,
									y: 0,
									x2: 0,
									y2: 1,
									colorStops: [
										{
											offset: 0,
											color: "rgba(116,145,214,0.4)", // 0% 处的颜色
										},
										{
											offset: 0.7,
											color: "rgba(116,145,214,0)", // 100% 处的颜色
										},
									],
									globalCoord: false, // 缺省为 false
								},
							},
						},
					},
				],
			});
		},
		echart6() {
			var x1 = [];
			var baty1 = [];
			var caby1 = [];
			var cary1 = [];
			var x2 = [];
			var baty2 = [];
			var caby2 = [];
			var cary2 = [];
			var x3 = [];
			var baty3 = [];
			var caby3 = [];
			var cary3 = [];
			Api.getModuleData({ moduleName: "30_day_notOnline" })
				.then((res) => {
					if (res.success) {
						this.day30Data = res.items;
						for (var j in this.day30Data) {
							if (this.day30Data[j].childrenModuleName == "not_online_battery") {
								for (var i in this.day30Data[j].list[0].list) {
									baty1.push(this.day30Data[j].list[0].list[i].showData01);
								}
								for (var i in this.day30Data[j].list[1].list) {
									baty2.push(this.day30Data[j].list[1].list[i].showData01);
								}
								for (var i in this.day30Data[j].list[2].list) {
									baty3.push(this.day30Data[j].list[2].list[i].showData01);
								}
							}
							if (this.day30Data[j].childrenModuleName == "not_online_cabinet") {
								for (var i in this.day30Data[j].list[0].list) {
									caby1.push(this.day30Data[j].list[0].list[i].showData01);
								}
								for (var i in this.day30Data[j].list[1].list) {
									caby2.push(this.day30Data[j].list[1].list[i].showData01);
								}
								for (var i in this.day30Data[j].list[2].list) {
									caby3.push(this.day30Data[j].list[2].list[i].showData01);
								}
							}
							if (this.day30Data[j].childrenModuleName == "not_online_car") {
								for (var i in this.day30Data[j].list[0].list) {
									cary1.push(this.day30Data[j].list[0].list[i].showData01);
								}
								for (var i in this.day30Data[j].list[1].list) {
									cary2.push(this.day30Data[j].list[1].list[i].showData01);
								}
								for (var i in this.day30Data[j].list[2].list) {
									cary3.push(this.day30Data[j].list[2].list[i].showData01);
								}
							}
						}
						for (var i in this.day30Data[0].list[0].list) {
							x1.push(this.day30Data[0].list[0].list[i].childrenModuleName);
						}
						for (var i in this.day30Data[0].list[1].list) {
							x2.push(this.day30Data[0].list[1].list[i].childrenModuleName);
						}
						for (var i in this.day30Data[0].list[2].list) {
							x3.push(this.day30Data[0].list[2].list[i].childrenModuleName);
						}
						this.Warning30(x1, baty1, caby1, cary1);
						setInterval(() => {
							this.circle1++;
							if (this.circle1 == 4) {
								this.circle1 = 1;
							}
							if (this.circle1 == 1) {
								this.Warning30(x3, baty3, caby3, cary3);
							} else if (this.circle1 == 2) {
								this.Warning30(x2, baty2, caby2, cary2);
							} else if (this.circle1 == 3) {
								this.Warning30(x1, baty1, caby1, cary1);
							}
						}, 8000);
					} else {
						this.$message.error(res.message);
					}
				})
				.catch(() => {});
		},
		Warning30(x, bat, cab, car) {
			var myChart4 = this.$echarts.init(document.getElementById("Chart4"));
			myChart4.clear();
			myChart4.setOption({
				grid: {
					top: "22%",
					left: "0%",
					right: "5%",
					bottom: "8%",
					containLabel: true,
				},
				tooltip: {
					trigger: "axis",
					axisPointer: {
						type: "cross",
					},
				},
				legend: {
					x: "center",
					data: [{ name: "车辆" }, { name: "电池" }, { name: "电柜" }],
					textStyle: {
						fontSize: "16",
						color: "#7491D6",
					},
				},
				xAxis: [
					{
						type: "category",
						data: x,
						axisPointer: {
							type: "shadow",
						},
						axisLabel: {
							show: true,
							textStyle: {
								color: "#7491D6", //更改坐标轴文字颜色
								fontSize: 16, //更改坐标轴文字大小
							},
						},
					},
				],
				yAxis: [
					{
						type: "value",
						name: "",
						axisLabel: {
							show: true,
							textStyle: {
								color: "#7491D6", //更改坐标轴文字颜色
								fontSize: 16, //更改坐标轴文字大小
							},
						},
						axisTick: {
							show: false,
						},

						axisLine: {
							lineStyle: {
								color: "#fff",
								opacity: 0,
							},
						},
						splitLine: {
							lineStyle: {
								color: "#658CE6",
								opacity: 0.4,
							},
						},
					},
				],
				series: [
					{
						name: "车辆",
						type: "line",
						data: car,
						itemStyle: {
							normal: {
								label: {
									show: false,
									position: "top",
									textStyle: {
										color: "white",
										fontSize: 10,
									},
								},
								color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [
									{ offset: 0, color: "#FFB543" },
									{ offset: 1, color: "#FFB543" },
								]),
							},
						},
					},
					{
						name: "电池",
						type: "line",
						data: bat,
						itemStyle: {
							normal: {
								color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [
									{ offset: 0, color: "#3184FE" },
									{ offset: 1, color: "#3184FE" },
								]),
							},
						},
					},
					{
						name: "电柜",
						type: "line",
						data: cab,
						itemStyle: {
							normal: {
								color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [
									{ offset: 0, color: "#1BE08A" },
									{ offset: 1, color: "#1BE08A" },
								]),
							},
						},
					},
				],
			});
		},
		echart7() {
			var x1 = [];
			var y1 = [];
			var x2 = [];
			var y2 = [];
			Api.getModuleData({ moduleName: "device_fault" })
				.then((res) => {
					if (res.success) {
						this.fault = res.items;
						for (var i in this.fault) {
							if (this.fault[i].childrenModuleName == "todat_cebinet") {
								this.todat_cebinet = this.fault[i].showData01;
							}
							if (this.fault[i].childrenModuleName == "today_battery") {
								this.today_battery = this.fault[i].showData01;
							}
							if (this.fault[i].childrenModuleName == "30_day_bettery") {
								for (var j in this.fault[i].list) {
									x1.push(this.fault[i].list[j].childrenModuleName);
									y1.push(this.fault[i].list[j].showData01);
								}
							}
							if (this.fault[i].childrenModuleName == "30_day_cebinet") {
								for (var j in this.fault[i].list) {
									x2.push(this.fault[i].list[j].childrenModuleName);
									y2.push(this.fault[i].list[j].showData01);
								}
							}
						}
						// console.log(x1);
						// console.log(y1);
						// console.log(x2);
						// console.log(y2);
						var myChart5 = this.$echarts.init(document.getElementById("Chart5"));
						var myChart6 = this.$echarts.init(document.getElementById("Chart6"));
						myChart5.clear();
						myChart6.clear();
						myChart5.setOption({
							grid: {
								top: "0%",
								left: "-25%",
								right: "10%",
								bottom: "-15%",
								containLabel: true,
							},
							xAxis: {
								axisLabel: {
									show: false,
								},
								axisTick: {
									show: false,
								},
								type: "category",
								boundaryGap: false,
								data: x1,
							},
							yAxis: {
								axisLabel: {
									show: false,
								},
								splitLine: {
									show: false,
								},
								axisTick: {
									show: false,
								},
								type: "value",
							},
							series: [
								{
									symbol: "none",
									smooth: true, //让曲线变平滑的
									data: y2,
									type: "line",
									itemStyle: {
										normal: {
											color: "#FFEA00",
										},
									},
									areaStyle: {
										normal: {
											color: "#FFEA00", //改变区域颜色
											opacity: 0.2,
										},
									},
								},
							],
						});
						myChart6.setOption({
							grid: {
								top: "0%",
								left: "-25%",
								right: "10%",
								bottom: "-15%",
								containLabel: true,
							},
							xAxis: {
								axisLabel: {
									show: false,
								},
								axisTick: {
									show: false,
								},
								type: "category",
								boundaryGap: false,
								data: x2,
							},
							yAxis: {
								axisLabel: {
									show: false,
								},
								splitLine: {
									show: false,
								},
								axisTick: {
									show: false,
								},
								type: "value",
							},
							series: [
								{
									symbol: "none",
									smooth: true, //让曲线变平滑的
									data: y2,
									type: "line",
									itemStyle: {
										normal: {
											color: "#D2014C",
										},
									},
									areaStyle: {
										normal: {
											color: "#D2014C", //改变区域颜色
											opacity: 0.2,
										},
									},
								},
							],
						});
					} else {
						this.$message.error(res.message);
					}
				})
				.catch(() => {});
		},
		// 获得中国地图参数
		getChinaData() {
			return new Promise((resolve) => {
				Api.getNumByProvince().then((res) => {
					if (res.success) {
						let data = [];
						this.detailData = res.items;
						this.provinceData = res.items;
						this.proNum = res.items;
						for (var i in this.proNum) {
							this.proNum[i].provinceName == null ? (this.proNum[i].provinceName = "") : this.proNum[i].provinceName;
							this.proNum[i].lon == null ? (this.proNum[i].lon = 0) : this.proNum[i].lon;
							this.proNum[i].lat == null ? (this.proNum[i].lat = 0) : this.proNum[i].lat;
							data[i] = { name: "", value: [] };
							var arr = [];
							var num = this.proNum[i].bNum + this.proNum[i].cbNum + this.proNum[i].vNum;
							arr.push(this.proNum[i].lon, this.proNum[i].lat, num);
							data[i].name = this.proNum[i].provinceName;
							data[i].value = arr;
						}
						resolve(data);
					}
				});
			});
		},
		// 全屏点击
		handleFullScreen() {
			var el = document.getElementById("new-screen");
			if (this.isFullScreen()) {
				this.exitFullscreen();
			} else {
				this.full(el);
			}
		},
		// 进入全屏
		full(ele) {
			if (ele.requestFullscreen) {
				ele.requestFullscreen();
			} else if (ele.mozRequestFullScreen) {
				ele.mozRequestFullScreen();
			} else if (ele.webkitRequestFullscreen) {
				ele.webkitRequestFullscreen();
			} else if (ele.msRequestFullscreen) {
				ele.msRequestFullscreen();
			}
		},
		// 是否是全屏状态
		isFullScreen() {
			return !!(document.fullscreen || document.mozFullScreen || document.webkitIsFullScreen || document.webkitFullScreen || document.msFullScreen);
		},
		// 退出全屏
		exitFullscreen() {
			if (document.exitFullscreen || document.webkitExitFullscreen) {
				document.exitFullscreen();
			} else if (document.msExitFullscreen) {
				document.msExitFullscreen();
			} else if (document.mozCancelFullScreen) {
				document.mozCancelFullScreen();
			} else if (document.webkitExitFullscreen) {
				document.webkitExitFullscreen();
			}
		},
		// 当前时间
		NowTime() {
			//获取年月日
			var time = new Date();
			var year = time.getFullYear();
			var month = time.getMonth() + 1;
			var day = time.getDate();

			//获取时分秒
			var h = time.getHours();
			var m = time.getMinutes();
			var s = time.getSeconds();

			//检查是否小于10
			month = this.check(month);
			day = this.check(day);
			h = this.check(h);
			m = this.check(m);
			s = this.check(s);
			this.time = year + "." + month + "." + day + "    " + h + ":" + m + ":" + s;
		},
		// 补0
		check(i) {
			var num;
			i < 10 ? (num = "0" + i) : (num = i);
			return num;
		},
		thousandNum(num) {
			return num.toString().replace(/\d+/, function (n) {
				// 先提取整数部分
				return n.replace(/(\d)(?=(\d{3})+$)/g, function ($1) {
					// 对整数部分添加分隔符
					return $1 + ",";
				});
			});
		},
	},
	async mounted() {
		this.echart1();
		this.echart2();
		this.echart3();
		this.echart4();
		this.echart5();
		this.echartpatch();
		// setInterval(() => {
		// 	this.echart1();
		// 	this.echart2();
		// 	this.echart3();
		// 	this.echart4();
		// 	this.echartpatch();
		// }, 15000),
		this.echart6();
		this.echart7();
		this.setSelCity();
		this.initBottomRankOpt();
		setTimeout(() => {
			this.handleSelCity("上海市");
		}, 0);
	},
	created() {
		setInterval(() => {
			this.NowTime();
		}, 1000);
	},
};
</script>

<style scoped lang="scss">
.app-container {
	width: 100vw;
	height: 56.25vw;
	padding: 1%;
	padding-top: 1%;
	// background-image: url('./img/new-bg.png');
	background-image: url("https://zhizuxia-sit.obs.cn-north-1.myhuaweicloud.com/tgmini-img/boss-new-bg.png");
	background-repeat: no-repeat;
	background-size: 100% 100%;
	display: flex;
	// flex-direction: column;
	position: relative;

	#time {
		position: absolute;
		color: #fff;
		width: 30vw;
		height: 3vw;
		line-height: 3vw;
		text-align: left;
		right: 19%;
		font-size: 2vw;
		font-family: "ds";
	}
	.full-screen-btn {
		width: 2.5vw;
		height: 2.5vw;
		background-image: url("./img/new-btn.png");
		background-repeat: no-repeat;
		background-size: 100% 100%;
		cursor: pointer;
		position: absolute;
		left: 2vw;
	}
	.left-col,
	.right-col {
		width: 25vw;
		// height: 52vw;
		padding-top: 3vw;
		display: flex;
		flex-direction: column;
		.bg {
			background-image: url("./img/new-modal.png");
			background-repeat: no-repeat;
			background-size: 100% 100%;
			height: 30vh;
			width: 100%;
			position: relative;
			.title-name {
				color: #fff;
				// width: 27vw;
				height: 2.3vw;
				font-size: 1.1vw;
				text-align: center;
				display: flex;
				align-items: center;
				justify-content: center;
				font-weight: 500;
			}
		}
		.right-2 {
			background-image: url("./img/right-2.png");
		}
		.top {
			position: relative;
			.content {
				height: 90%;
				width: 100%;
				// padding: 0 1.1vw;
			}
			#left-top-1 {
				width: 100% !important;
				height: 280px !important;
			}
			.right-list {
				position: absolute;
				top: 53%;
				color: #fff;
				transform: translate(120%, -50%);
				width: 42%;
				background: rgba(37, 58, 104, 0.9);
				font-size: 0.95vw;
				.right-item {
					display: flex;
					align-items: center;
					height: 2vw;
					padding-left: 0.8vw;
					// justify-content: space-between;
					.right-title {
						display: flex;
						align-items: center;
						width: 6.8vw;
						font-family: PingFang SC;
					}
					.rect-icon {
						width: 0.6vw;
						height: 0.6vw;
						background: #fff;
						margin-right: 0.5vw;
					}
					.num {
						font-family: DIN Alternate;
					}
				}
			}
		}
		.center {
			.content {
				display: flex;
				justify-content: center;
				// height: calc(100% - 2vw);
				height: 90%;
				padding: 0 1%;
				.left-content,
				.right-content {
					width: 44%;
					height: 100%;
					background-size: cover;
					display: flex;
					flex-direction: column;
					justify-content: center;
					align-items: center;
					color: #7491d6;
					font-size: 0.95vw;
					.bg-img {
						position: relative;
						width: 8vw;
						height: 16vh;
						background-image: url("./img/show-bg.png");
						background-size: 100% 100%;
						margin-bottom: 10px;
						font-size: 2.2vw;
						font-weight: bold;
						.num {
							font-family: DIN Alternate;
							position: absolute;
							top: 63%;
							left: 50%;
							transform: translate(-50%, -50%);
						}
					}
				}
			}
		}
		.bottom {
			background-image: none;
			color: #7491d6;
			.content {
				height: 90%;
				height: calc(100% - 2vw);
				padding: 0 1vw;
				.ratio-list {
					width: 100%;
					padding-bottom: 3vh;
					.ratio-item {
						display: flex;
						align-items: center;
						padding-left: 2vw;
						margin-bottom: 1.4vh;
						width: 100%;
						> div {
							width: 4.5vw;
							font-size: 0.95vw;
						}
						/deep/ .el-progress-bar__outer {
							height: 1vh !important;
							background-color: #344d85 !important;
							border-radius: 0px;
						}
						/deep/ .el-progress-bar__inner {
							border-radius: 0 !important;
						}
						/deep/ .el-progress__text {
							color: #ffffff;
							font-size: 0.95vw !important;
							margin-right: 0.4vw;
						}
					}
				}
			}

			.line {
				display: flex;
				align-items: center;
				// justify-content: space-between;
				.center {
					width: 40%;
					margin: 0 1vw;
				}
				.center,
				.right {
					& p:first-child {
						font-size: 2.2vw;
						margin-bottom: 1vw;
						font-family: DIN Alternate;
						text-align: left;
					}
					& p:last-child {
						font-size: 0.95vw;
						text-align: left;
					}
				}
			}
		}
		.right-bg {
			background-image: url("./img/new-modal.png");
		}
	}
	.center-col {
		width: 50vw;
		position: relative;
		display: flex;
		justify-content: center;
		> .title {
			color: #fff;
			height: 6vw;
			line-height: 6vw;
			text-align: right;
			width: 38vw;
			font-size: 1.7vw;
		}
		.num-list-show {
			display: flex;
			justify-content: space-evenly;
			width: 100%;
			position: absolute;
			top: 10vh;
			color: #fff;
			z-index: 2021;
			.num-itme {
				> .title {
					text-align: center;
					font-size: 24px;
				}
				.num {
					font-size: 62px;
					font-family: "DIN Alternate";
					font-weight: bold;
					text-align: center;
					margin-top: 3.5vh;
					text-shadow: 0px 0px 38px rgba(146, 156, 250, 0.6);
				}
			}
		}
		.city-select {
			position: absolute;
			top: 24vh;
			left: 3vw;
			z-index: 2021;
			> .el-select {
				width: 5vw;
				/deep/ input {
					height: 3.5vh;
				}
				/deep/ .selectFrom {
					margin-top: 0 !important;
					background: #132347 !important;
					border: none;

					/deep/ .popper__arrow {
						display: none !important;
					}
					.el-select-dropdown__empty {
						color: #fff;
					}
					.el-select-dropdown__item:hover {
						background-color: #18284a;
						color: #fff;
						border: 1px solid #658de7;
					}
					.el-select-dropdown__item {
						color: #fff;
					}
					.el-select-dropdown__item.selected {
						color: #fff;
					}
				}
				.el-select-dropdown__item.hover,
				.el-select-dropdown__item:hover {
					background-color: #132347 !important;
				}
			}

			/deep/ .el-icon-arrow-up:before {
				color: #658de7;
			}
			/deep/ input {
				color: #fff;
				background: none;
				border-color: #658de7 !important;
				box-shadow: rgb(101, 141, 231) 0px 0px 10px inset;
			}
			/deep/ input::-webkit-input-placeholder {
				color: #fff;
			}
		}
		.bottom-city-rank {
			position: absolute;
			bottom: 8vh;
			z-index: 2021;
			width: 80%;

			.city-list {
				display: flex;
				justify-content: space-between;
				.city-item {
					width: 12vw;
					height: 22vh;
					background-image: url("./img/bottom-box.png");
					background-size: 100% 100%;
					background-repeat: no-repeat;
					display: flex;
					justify-content: center;
					align-items: center;
					position: relative;
					.click-mask {
						position: absolute;
						left: 0;
						top: 0;
						bottom: 0;
						right: 0;
						color: #b3d5fb;
						display: flex;
						justify-content: center;
						align-items: center;
						cursor: pointer;
					}
				}
			}
		}
		#map {
			position: absolute;
			bottom: 3vh;
			left: 0;
		}
		.bottom-bg {
			margin: 0 auto;
			width: 80%;
			height: 8vh;
			line-height: 8vh;
			position: absolute;
			bottom: 0;
			background-image: url("./img/bottom-bg.png");
			background-size: 100% 100%;
			background-repeat: no-repeat;
			color: #fef9ef;
			text-align: center;
			font-size: 42px;
			text-shadow: 0px 0px 38px rgba(146, 156, 250, 0.6);
			font-style: italic;
		}
		.add-length{
			width: 110%;
		}
	}
}
</style>
