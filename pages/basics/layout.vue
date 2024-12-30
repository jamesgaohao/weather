<template>
	<view>
		
		<cu-custom bgColor="bg-gradual-blue" :isBack="true">
			<block slot="content">煎蛋天气</block>
		</cu-custom>
		<scroll-view scroll-x class="bg-white nav text-center fixed" :style="[{top:CustomBar + 'px'}]">
			<view style="font-size: 120%;" class="cu-item" :class="index==TabCur?'text-blue cur':''" v-for="(item,index) in tabNav" :key="index" @tap="tabSelect"
			 :data-id="index">
				{{tabNav[index]}}
			</view>
		</scroll-view>
		<block v-if="TabCur==0">
			<view class=" animation-slide-top padding bg-white" style="animation-delay: 0.5s;height: 110vh;width:100vw;">
				<view class="flex">
					<view class="flex-sub bg-white padding-sm margin-xs radius">
						<view>
							<image style="height: 160upx; width: 160upx;"src="../../static/location_red.gif"></image>
							<text style="font-size: 120%;">{{addres}} </text>
							<view style="margin-top: 15%;"></view>
							<view v-if="warnings.length > 0 ">
								<image  src="../../static/import.gif" style="height: 160upx; width: 160upx;margin-right: 10%; float: right;"></image>
								<swiper
								    circular="true" 
								    vertical="true" 
								    autoplay="true" 
								    interval="3000" 
								    duration="1000"
								>
								    <swiper-item v-for="(item,index) in warnings" :key="index">    
										{{item.text}}	
									</swiper-item> 
								</swiper>
								
							</view>
							<view v-else> 
							    <image src="../../static/littleyellowduck.gif" style="height: 160upx; width: 160upx;margin-left: 1%;"></image>
								<image src="../../static/littleyellowduck.gif" style="height: 160upx; width: 160upx;margin-left: 1%;float: right;"></image>
							</view>
							<i :class="'qi-'+ now.icon" style="margin-left: 20%; font-size: 520%; color: #e54d42;font-weight: 1000;"></i>
							<i style="margin-left: 5%;font-size: 200%;">{{now.text}}</i>
							<i style="margin-left: 5%; font-size: 200%;">{{now.temp}}°</i>	
						</view>
					</view>
				</view>
				<view class=" background-image-container text-left" style="font-size: 120%;color: #666666;border: 6px solid #FCE4EC;border-style: inset;">
					<view class="title" style="margin-top: 45%;margin-left: 29%;">观测时间: {{dateCover(now.obsTime)}}</view>
					<view class="title" style="margin-top: 20rpx;margin-left: 25%;">温度:<text style="margin-left: 2%;">{{now.temp}}°</text></view>
					<view class="title" style="margin-top: 20rpx;margin-left: 25%;">体感温度:<text style="margin-left: 2%">{{now.feelsLike}}°</text></view>
					<view class="title" style="margin-top: 20rpx;margin-left: 25%;">风向:<text style="margin-left: 2%;">{{now.windDir}}</text></view>
					<view class="title" style="margin-top: 20rpx;margin-left: 25%;">相对湿度:<text style="margin-left: 2%;">{{now.humidity}}</text>%</view>
					<view class="title" style="margin-top: 20rpx;margin-left: 25%;">风力等级:<text style="margin-left: 2%;">{{now.windScale}}</text>级</view>
				</view>
			</view>		
		</block>
		<block v-if="TabCur==1">
			<view class=" animation-slide-top padding bg-white" style="animation-delay: 0.5s">
				<view class="animation-slide-top  cu-bar bg-white  margin-top solid-bottom">
					<view class="action">
						<text class="cuIcon-title text-blue"></text>空气质量
					</view>
					<image style="height: 128rpx; width: 128rpx;" src="../../static/poppy.gif"></image>
				</view>
				<view class=" animation-slide-top bg-white" style="animation-delay: 0.5s;font-size: 120%;">
					<view style="margin-top: 8%; margin-bottom: 8%;">
					<view class="flex  p-xs margin-bottom-sm mb-sm">
						<view class="flex-twice  padding-sm margin-xs radius">空气质量&nbsp;:&nbsp;{{hoursAirQuality[0].indexes[0].category}}</view>
						<view class="flex-treble  padding-sm margin-xs radius">空气数值&nbsp;:&nbsp;{{hoursAirQuality[0].indexes[0].aqi}}</view>
					</view>
					<view class="flex  p-xs margin-bottom-sm mb-sm">
						<view class="flex-twice  padding-sm margin-xs radius">空气质量等级&nbsp;:&nbsp;{{hoursAirQuality[0].indexes[0].level}}</view>
						<view v-if="hoursAirQuality[0].indexes[0].primaryPollutant">
							<view class="flex-treble  padding-sm margin-xs radius">首要污染物名称</view>
							<view  class="basis-lg  margin-xs padding-sm radius">{{hoursAirQuality[0].indexes[0].primaryPollutant.name}}</view>
						</view> 
						 <view v-else> </view>
					</view>
					</view>
				</view>
				<view style="font-size: 120%;">
					健康活动建议：<br>
					<br>
					<text style="margin-left: 5%;">{{hoursAirQuality[0].indexes[0].health.effect}}</text><br>
					<text>{{hoursAirQuality[0].indexes[0].health.advice.generalPopulation}}</text><br>
					<text style="margin-left: 5%;">{{hoursAirQuality[0].indexes[0].health.advice.sensitivePopulation}}</text>
					
				</view>
				<view class="container">
				    <scroll-view class="scroll-view_H" scroll-x="true" style="width: 100%;height: 200rpx;">
				      <view class="item" v-for="(item, index) in hoursAirQuality" :key="index">
						 <view :style='getBackgroundColor(item)'>
							 <text style="font-size: 120%;">{{dateCover(item.forecastTime)}}</text><br/>
							<text style="font-size: 120%;">污染程度:{{item.indexes[0].category}} </text><br/>
							 <text style="font-size: 120%;">空气质量数值:{{item.indexes[0].aqi}}</text><br/>
						 </view>
				      </view>
				    </scroll-view>
				  </view>						
				<view class="animation-slide-top  cu-bar bg-white  margin-top solid-bottom">
					<view class="action">
						<text class="cuIcon-title text-blue"></text>天气指数
					</view>
					<image style="height: 128rpx; width: 128rpx;" src="../../static/flower.gif"></image>
				</view>
				<view class=" animation-slide-top bg-white" style="animation-delay: 0.5s;height: 800rpx;">
					<view class="cu-item" v-for="(item,index) in indices" :key="index">
				    <view style="padding-top: 2%;"></view>	
					<form class="bg-white" style="font-size: 122%;">
						<view class="shadow light " style="margin-top: 2%; margin-left: 25rpx;padding-top: 5%; width: 680rpx;">
							<!-- <text style="margin-left: 3%;margin-right: 1%;" >{{item.date}}</text><br> -->
							<text style="margin-left: 4%;margin-right: 1%;">{{item.name}}:</text><br>
							<text style="margin-left: 5%;margin-right: 1%;"></text>{{item.category}}°
							<text style="margin-left: 1%;margin-right: 1%;"></text>{{item.text}}°
						 </view>
					</form>	
					</view>
				</view> 
			</view>
		</block>
		<block v-if="TabCur==2">
			<view class="animation-slide-top  bg-white" style="animation-delay: 0.5s;">
				<view class="animation-slide-top  cu-bar bg-white  margin-top solid-bottom">
					<view class="action">
						<text class="cuIcon-title text-blue"></text>分钟级降水（未来2小时每5分钟降雨预报数据）
					</view>
				</view>
				<view v-if='minutelyDate.summary ==="未来两小时无降水"' >
					<view class="background-image" style="background-image: url('../../static/springFlower.gif'); margin-top: 5%;  margin-left: 150rpx; height: 300rpx; width: 400rpx; border-radius: 20%; border: 6px solid powderblue;">
						<view style="margin-top: 25%; text-align: center;">
							<text>{{minutelyDate.summary}}</text>
						</view>
					</view>
				</view>
			    <view v-else style="background-image: url('../../static/raining.gif')">
						<view style="margin-left: 35%; padding-top:2%;">
							<image style="height: 180rpx; width: 200rpx;" src="../../static/rainNature.gif"></image>
						</view>
						<view style="margin-left: 150rpx; height: 200rpx; width: 500rpx; border-radius: 20%; border: 6px solid powderblue;">
							<view style="margin-top: 12%; text-align: center;">
								<text>{{minutelyDate.summary}}</text>
							</view>
						</view>
						<scroll-view class="list" scroll-y>
						<view v-for="(item,index) in minutelyDate.minutely" :key="index" >
							<view class="cu-timeline">
								<view class="cu-time">{{dateCover(item.fxTime)}}</view>
								<view class="cu-item text-blue">
									<view class="content bg-blue shadow-blur">
										<text></text>  {{item.type}} {{item.precip}}
									</view>
								</view>
							</view>
						</view>
						</scroll-view>	
				</view>
				<view>
					<view class="animation-slide-top  cu-bar bg-white  margin-top solid-bottom">
						<view class="action">
							<text class="cuIcon-title text-blue"></text>未来7天-天气预报
							<image style="height: 128rpx; width: 128rpx; float: left;" src="../../static/windmill.gif"></image>
						</view>
						
					</view>
					<view class=" animation-slide-top bg-white" style="animation-delay: 0.5s;">
						<scroll-view class="list" scroll-y="true">
							<view class="cu-item" v-for="(item,index) in day7s" :key="index">
							<view style="padding-top: 2%;"></view>	
							<form class="light  bg-white " style="font-size: 122%;">
								<view class="shadow light " style="margin-top: 2%; margin-left: 25rpx;padding-top: 5%;">
									<text>{{item.fxDate}}</text><br>
									<text style="margin-left: 5%;margin-right: 1%;">最高温度:</text>{{item.tempMax}}°
									<text style="margin-left: 1%;margin-right: 1%;">最低温度:</text>{{item.tempMin}}°
									<text style="margin-left: 1%;margin-right: 1%;"></text>{{item.textDay}}
									<i :class="'qi-'+item.iconDay" style=" margin-left: 4%;font-size: 125%; ;font-weight: 1000;"></i>
								 </view>
							</form>	
							</view>
						</scroll-view>
					</view>
				</view>
			</view>	
		</block>	
	</view>
</template>

<script>
	import moment from 'moment';
	export default {
		data() {
			return {
				key: '65928ab2ac0ae3beb21201cd0a31c35d', 
				amapPlugin: null,  
				isVisible: false,
				latitude:'',
				longitude:'',
				addres: '',
				CustomBar: this.CustomBar, 
				TabCur: 0,
				avatar: ['https://ossweb-img.qq.com/images/lol/web201310/skin/big10001.jpg',
					'https://ossweb-img.qq.com/images/lol/web201310/skin/big81005.jpg',
					'https://ossweb-img.qq.com/images/lol/web201310/skin/big25002.jpg',
					'https://ossweb-img.qq.com/images/lol/web201310/skin/big99008.jpg'
				], 
				warnings: [],
				tabNav: ['实时天气','天气指数','未来天气预报'],
				day7s:[ {
						  "fxDate": "",
						  "sunrise": "",
						  "sunset": "",
						  "moonrise": "",
						  "moonset": "",
						  "moonPhase": "",
						  "moonPhaseIcon": "",
						  "tempMax": "",
						  "tempMin": "",
						  "iconDay": "",
						  "textDay": "",
						  "iconNight": "",
						  "textNight": "",
						  "wind360Day": "",
						  "windDirDay": "",
						  "windScaleDay": "",
						  "windSpeedDay": "",
						  "wind360Night": "",
						  "windDirNight": "",
						  "windScaleNight": "",
						  "windSpeedNight": "",
						  "humidity": "",
						  "precip": "",
						  "pressure": "",
						  "vis": "",
						  "cloud": "",
						  "uvIndex": ""
						}],
			     now: {
					"obsTime": "",
					"temp": "",
					"feelsLike": "",
					"icon": "",
					"text": "",
					"wind360": "",
					"windDir": "",
					"windScale": "",
					"windSpeed": "",
					"humidity": "",
					"precip": "",
					"pressure": "",
					"vis": "",
					"cloud": "",
					"dew": ""
				  },
				indices: [
						{
						  "date": "",
						  "type": "",
						  "name": "",
						  "level": "",
						  "category": "",
						  "text": ""
						}
					  ],
				hoursAirQuality: [
									{
										"forecastTime": "",
										"indexes": [
											{
												"code": "",
												"name": "",
												"aqi": '',
												"aqiDisplay": "",
												"level": "",
												"category": "",
												"color": {
													"red": '',
													"green": '',
													"blue": '',
													"alpha": ''
												},
												"primaryPollutant": {
													"code": "",
													"name": "",
													"fullName": ""
												},
												"health": {
													"effect": "",
													"advice": {
														"generalPopulation": null,
														"sensitivePopulation": ""
													}
												}
											}
										],
										"pollutants": []
									}],
				minutelyDate: {
					"updateTime": "",
					"fxLink": "",
					"summary": "",
					"minutely": [{
							"fxTime": "",
							"precip": "",
							"type": ""
						}
					],

				},


			};
		},
		onPullDownRefresh() {
			 // 模拟数据更新
			     setTimeout(() => {
					
			       // 停止下拉刷新动画
				   // this.tabSelect(currentTabIndex);
				   // 页面切换到指数的时候调用接口
				   if(this.TabCur===1){
				   	// 获取实时空气质量指数
				   	this.qweatherAirquality();
				   	// 获取天气指数
				   	this.qweatherIndices();
				   }
				   // 未来天气预报
				   if(this.TabCur === 2){
				   	// 降雨接口调用
				   	this.qweather5minutely();
				   	// 7天预报接口调用
				   	this.qweatherDay7s();
				   	
				   }
				   if(this.TabCur === 0){
				   	// 调用今天的天气实时数据
				   	this.qweatherNow(); // 确保在 getlocation 完成后再调用 qweatherNow
				   	this.getAddress(uni.getStorageSync("location"));
				   }
				
			       uni.stopPullDownRefresh();
			     }, 1000);
			 
			 
		 },
	onLoad: function (options) {
		        console.log("开始调用接口")
				// 调用今天的天气实时数据
				this.getlocation().then(() => {
				        this.qweatherNow(); // 确保在 getlocation 完成后再调用 qweatherNow
						this.getAddress(uni.getStorageSync("location"))
						
				    }).catch(error => {
				       
				    });
				
			},
			mounted() {
			
			},
		methods: {
			getParaIfNull(para) {
			    if (para === null || para === undefined || para === "") {
			        return "无"; // 返回一个更有意义的默认值
			    }
			    return para; // 如果 para 有值，直接返回
			},
			tabSelect(e) {
				console.log("TabCur======"+JSON.stringify(this.TabCur))
				this.TabCur = e.currentTarget.dataset.id;
				this.latitude=uni.getStorageSync("latitude");
				this.longitude=uni.getStorageSync("longitude");
				
				// 页面切换到指数的时候调用接口
				if(this.TabCur===1){
					// 获取实时空气质量指数
					this.qweatherAirquality();
					// 获取天气指数
					this.qweatherIndices();
				}
				// 未来天气预报
				if(this.TabCur === 2){
					// 降雨接口调用
					this.qweather5minutely()
					// 7天预报接口调用
					this.qweatherDay7s();
					
				}
				if(this.TabCur === 0){
					// 调用今天的天气实时数据
					this.qweatherNow(); // 确保在 getlocation 完成后再调用 qweatherNow
					this.getAddress(uni.getStorageSync("location"))
				}
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60
			},
			dateCover(isoString){
				const m = moment(isoString);
				return m.format('YYYY-MM-DD HH:mm')
			},
			 getBackgroundColor(item) {
			      const redValue = item.indexes[0].color.red || 0; // 防止未定义的情况
			      const greenValue = item.indexes[0].color.green || 0; // 防止未定义的情况
			      const blueValue = item.indexes[0].color.blue || 0; // 防止未定义的情况
				  const alphaValue = item.indexes[0].color.alpha || 0
			      return `background-color: rgb(${redValue},${greenValue},${blueValue},${alphaValue}); border-radius: 20px;`;
			    },
			getlocation(){
				return new Promise((resolve, reject) => {
				      uni.getLocation({
				      	type: 'gcj02', //返回可以用于uni.openLocation的经纬度
						altitude: true,
						geocode:true ,
				      	success: function (res) {
							console.log("原始数据:"+JSON.stringify(res));
							console.log("原始数据latitude:"+res.latitude);
							console.log("原始数据longitude:"+res.longitude);
				      		this.latitude=res.latitude.toFixed(2);
				      		this.longitude=res.longitude.toFixed(2);
				      		console.log("进入到getlocation-成功获取到数据");
				      		uni.setStorageSync("latitude",this.latitude);
				      		uni.setStorageSync("longitude",this.longitude);
				      		console.log("latitude:"+this.latitude);
				      		console.log("longitude:"+this.longitude);
							const location=res.longitude+","+res.latitude
							console.log("location-组装======获取到数据"+location);
						    uni.setStorageSync("location",location);
							resolve();
				      	},
				      });
				    });
			},
			getAddress :function(location){
			   uni.request({
			     url: "https://restapi.amap.com/v3/geocode/regeo?key=f5d665fb2017c06166f39a009af91804&location="+location,
			     method: 'GET',
			     success: (res) => {
			   	  console.log("调用高德+++++=="+ JSON.stringify(res.data))
					if(res.data.status==='1'){
						 this.addres=res.data.regeocode.formatted_address
					}
			     },
			     fail: (err) => {
			       console.error(err);
			     },
			   });	
			       
			},
			// 获取实时天气数据
			qweatherNow(){
				uni.getLocation({
					type: 'gcj02', //返回可以用于uni.openLocation的经纬度
					geocode:true ,
					success: function (res) {
						console.log("原始数据:"+JSON.stringify(res));
						console.log("原始数据latitude:"+res.latitude);
						console.log("原始数据longitude:"+res.longitude);
						this.latitude=res.latitude.toFixed(2);
						this.longitude=res.longitude.toFixed(2);
						console.log("进入到getlocation-成功获取到数据");
						uni.setStorageSync("latitude",this.latitude);
						uni.setStorageSync("longitude",this.longitude);
						console.log("latitude:"+this.latitude);
						console.log("longitude:"+this.longitude);
						const location=res.longitude+","+res.latitude
						console.log("location-组装======获取到数据"+location);
						uni.setStorageSync("location",location);
						
					},
					fail: function (err) { 
						
						console.log("报错了"+JSON.stringify(err));
						
					}
				});
				
				
				//提交接口数据
				this.latitude=uni.getStorageSync("latitude");
				this.longitude=uni.getStorageSync("longitude");
				console.log("=======longitude:"+this.latitude)
				//提交接口数据
				uni.request({
				  // url: "https://devapi.qweather.com/v7/weather/now?location="+this.longitude+','+this.latitude,
				  url: "https://devapi.qweather.com/v7/weather/now?location="+this.longitude+','+this.latitude,
				  header:{
					  'X-QW-Api-Key': "4a8e33fed0884f3eb5f0dd56c0b00239",
				  },
				  method: 'GET',
				  success: (res) => {
					  console.log("+++++=="+ JSON.stringify(res.data))
					
					  if("200" === res.data.code){
						  this.now=res.data.now;
					  }
				  },
				  fail: (err) => {
				    console.error(err);
				  },
				});	
				 
			},
		
			// 获取7天气数据
			qweatherDay7s(){
				//提交接口数据
				uni.request({
				  url: "https://devapi.qweather.com/v7/weather/7d?location="+this.longitude+','+this.latitude,
				  // url: "http://192.168.1.2:9081/api/v1/user/applicationFrom/getCurrentApplication",
				  header:{
					  'X-QW-Api-Key': "4a8e33fed0884f3eb5f0dd56c0b00239",
				  },
				  method: 'GET',
				  success: (res) => {
					  console.log("获取7天气数据+++++=="+JSON.stringify(res.data))
					  this.day7s=res.data.daily
				  },
				  fail: (err) => {
				    console.error(err);
				  },
				});	
			},
			// 获取5分钟降雨预报数据
			qweather5minutely(){
				
				//提交接口数据
				uni.request({
				  url: "https://devapi.qweather.com/v7/minutely/5m?location="+this.longitude+','+this.latitude,
				  // url: "http://192.168.1.2:9081/api/v1/user/applicationFrom/getCurrentApplication",
				  header:{
					  'X-QW-Api-Key': "4a8e33fed0884f3eb5f0dd56c0b00239",
				  },
				  method: 'GET',
				  success: (res) => {
					  if("200" === res.data.code){
					  	  this.minutelyDate=res.data
					  }
					 
				  },
				  fail: (err) => {
				    console.error(err);
				  },
				});	
			},
			// 获取实时空气质量
			qweatherAirquality(){
				console.log("qweatherAirquality中的longitude"+this.longitude)
				//提交接口数据
				uni.request({
				  url: "https://devapi.qweather.com/airquality/v1/hourly/"+this.latitude+'/'+this.longitude,
				  // url: "http://192.168.1.2:9081/api/v1/user/applicationFrom/getCurrentApplication",
				  header:{
					  'X-QW-Api-Key': "4a8e33fed0884f3eb5f0dd56c0b00239",
				  },
				  method: 'GET',
				  success: (res) => {
					       console.log("qweatherAirquality中的"+JSON.stringify(res.data) )
						  if(res.data.hours.length > 0){
						  	this.hoursAirQuality=res.data.hours
						  }
				  },
				  fail: (err) => {
				    console.error(err);
				  },
				});	
			},
			// 获取实时空气指数
			qweatherIndices(){
				//提交接口数据
				uni.request({
				  url: "https://devapi.qweather.com/v7/indices/1d?type=1&location="+this.longitude+','+this.latitude,
				  // url: "http://192.168.1.2:9081/api/v1/user/applicationFrom/getCurrentApplication",
				  header:{
					  'X-QW-Api-Key': "4a8e33fed0884f3eb5f0dd56c0b00239",
				  },
				  method: 'GET',
				  success: (res) => {
					    console.log("qweatherIndices中的"+JSON.stringify(res.data) )
					  if("200" === res.data.code){
						   console.log("qweatherIndices==============")
					  	 this.indices=res.data.daily
					  }
					 
				  },
				  fail: (err) => {
				    console.error(err);
				  },
				});	
			},
			// 获取当前天气灾害预警
			qweatherWarning(){
				//提交接口数据
				uni.request({
				  url: "https://devapi.qweather.com/v7/warning/now?location="+this.longitude+','+this.latitude,
				  // url: "http://192.168.1.2:9081/api/v1/user/applicationFrom/getCurrentApplication",
				  header:{
					  'X-QW-Api-Key': "4a8e33fed0884f3eb5f0dd56c0b00239",
				  },
				  method: 'GET',
				  success: (res) => {
					  console.log("+++++=="+JSON.stringify(res.data))
					 
					  if("200" === res.data.code){
					  	this.warnings=res.data.warning
					  }
				  },
				  fail: (err) => {
				    console.error(err);
				  },
				});	
			},
			
			
		}
	}
</script>

<style>
	@import url("https://cdn.jsdelivr.net/npm/qweather-icons@1.3.0/font/qweather-icons.css");
	@import "../../colorui/animation.css";
	page {
		padding-top: 45px;
	}
	.list {
	    height: 700rpx;
	    overflow: hidden; /* 防止滚动时出现多余的滚动条 */
		border: 10px solid white;
		display: inline-block;
		overflow-y: auto; /* 允许纵向滚动 */
	}
	.title {
		margin-left: 8%;
	}
	
	.timeline-item {
	    display: flex;
	    align-items: center;
	    padding: 20rpx 0;
	}
	
	.cu-time {
	    width: 40%;
	    font-size: 125%; /* 调整为适合的字体大小 */
	    white-space: nowrap; /* 确保时间不会换行 */
	}
	
	.content {
	    padding: 10rpx 20rpx; /* 给内容添加内边距 */
	    border-radius: 8rpx; /* 圆角边框 */
	    font-size: 125%; /* 调整为适合的字体大小 */
	}
	
	.background-image-container {
	  /* 设置背景图片 */
	  background-image: url('../../static/nowcontent.gif');
	  /* 设置背景图片宽度自适应，且保持宽高比 */
	  background-size: cover;
	  /* 背景图片不重复 */
	  background-repeat: no-repeat;
	  /* 背景图片居中显示（可选） */
	  background-position: center;
	  /* 其他样式 */
	  width: 100%; /* 容器宽度 */
	  height: 50%;; /* 容器高度，根据需要设置 */
	}
	.background-image {
	  /* 设置背景图片宽度自适应，且保持宽高比 */
	  background-size: cover;
	  /* 背景图片不重复 */
	  background-repeat: no-repeat;
	  /* 背景图片居中显示（可选） */
	  background-position: center;
	  /* 其他样式 */
	  width: 100%; /* 容器宽度 */
	  height: 500px; /* 容器高度，根据需要设置 */
	}
	
	.scroll-view_H {
	  white-space: nowrap;
	  display: flex;
	}
	 
	.item {
	  display: inline-block;
	  width: 200px;
	  margin-right: 10px;
	  background-color: #fff;
	  text-align: center;
	  margin-top: 8%;
	}

	
</style>