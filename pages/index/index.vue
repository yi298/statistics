<template>
	<view class="qiun__body">
		<view class="qiun-columns">
			<view class="qiun-padding" style="font-size: 32upx;">
				<text>{{ tips }}</text>
			</view>
			<view class="qiun-bg-white qiun-title-bar qiun-common-mt"><view class="qiun-title-dot-light">今日计划量</view></view>
			<view class="qiun-charts">
				<!--#ifdef MP-ALIPAY -->
				<canvas
					canvas-id="canvasRing"
					id="canvasRing"
					class="charts"
					:width="cWidth * pixelRatio"
					:height="cHeight * pixelRatio"
					:style="{ width: cWidth + 'px', height: cHeight + 'px' }"
					@touchstart="touchPie($event, 'canvasRing')"
				></canvas>
				<!--#endif-->
				<!--#ifndef MP-ALIPAY -->
				<canvas canvas-id="canvasRing" id="canvasRing" class="charts" @touchstart="touchPie($event, 'canvasRing')"></canvas>
				<!--#endif-->
			</view>
		</view>
		<view class="qiun-columns">
			<view class="qiun-padding" style="font-size: 32upx;">
				<text>{{ tips }}</text>
			</view>
			<view class="qiun-bg-white qiun-title-bar qiun-common-mt"><view class="qiun-title-dot-light">昨日计划量</view></view>
			<view class="qiun-charts">
				<!--#ifdef MP-ALIPAY -->
				<canvas
					canvas-id="canvasRing2"
					id="canvasRing2"
					class="charts"
					:width="cWidth * pixelRatio"
					:height="cHeight * pixelRatio"
					:style="{ width: cWidth + 'px', height: cHeight + 'px' }"
					@touchstart="touchPie($event, 'canvasRing2')"
				></canvas>
				<!--#endif-->
				<!--#ifndef MP-ALIPAY -->
				<canvas canvas-id="canvasRing2" id="canvasRing2" class="charts" @touchstart="touchPie($event, 'canvasRing2')"></canvas>
				<!--#endif-->
			</view>
		</view>
		<view class="qiun-columns">
			<view class="qiun-padding" style="font-size: 32upx;">
				<text>{{ tips }}</text>
			</view>
			<view class="qiun-bg-white qiun-title-bar qiun-common-mt"><view class="qiun-title-dot-light">本周计划量</view></view>
			<view class="qiun-charts">
				<!--#ifdef MP-ALIPAY -->
				<canvas
					canvas-id="canvasRing3"
					id="canvasRing3"
					class="charts"
					:width="cWidth * pixelRatio"
					:height="cHeight * pixelRatio"
					:style="{ width: cWidth + 'px', height: cHeight + 'px' }"
					@touchstart="touchPie($event, 'canvasRing3')"
				></canvas>
				<!--#endif-->
				<!--#ifndef MP-ALIPAY -->
				<canvas canvas-id="canvasRing3" id="canvasRing3" class="charts" @touchstart="touchPie($event, 'canvasRing3')"></canvas>
				<!--#endif-->
			</view>
		</view>
		<view class="qiun-columns">
			<view class="qiun-padding" style="font-size: 32upx;">
				<text>{{ tips }}</text>
			</view>
			<view class="qiun-bg-white qiun-title-bar qiun-common-mt"><view class="qiun-title-dot-light">本月计划量</view></view>
			<view class="qiun-charts">
				<!--#ifdef MP-ALIPAY -->
				<canvas
					canvas-id="canvasRing4"
					id="canvasRing4"
					class="charts"
					:width="cWidth * pixelRatio"
					:height="cHeight * pixelRatio"
					:style="{ width: cWidth + 'px', height: cHeight + 'px' }"
					@touchstart="touchPie($event, 'canvasRing4')"
				></canvas>
				<!--#endif-->
				<!--#ifndef MP-ALIPAY -->
				<canvas canvas-id="canvasRing4" id="canvasRing4" class="charts" @touchstart="touchPie($event, 'canvasRing4')"></canvas>
				<!--#endif-->
			</view>
		</view>
	</view>
</template>

<script>
import uCharts from '@/js_sdk/u-charts/u-charts/u-charts.js';
var _self;
var canvasObj = {};

export default {
	data() {
		return {
			cWidth: '',
			cHeight: '',
			cWidth2: '', //横屏图表
			cHeight2: '', //横屏图表
			// cWidth3: '', //圆弧进度图
			// cHeight3: '', //圆弧进度图
			// arcbarWidth: '', //圆弧进度图，进度条宽度,此设置可使各端宽度一致
			// gaugeWidth: '', //仪表盘宽度,此设置可使各端宽度一致
			tips: '',
			pixelRatio: 1,
			serverData: '',
			// itemCount: 30, //x轴单屏数据密度
			// sliderMax: 50
		};
	},
	onLoad() {
		_self = this;
		//#ifdef MP-ALIPAY
		uni.getSystemInfo({
			success: function(res) {
				if (res.pixelRatio > 1) {
					//正常这里给2就行，如果pixelRatio=3性能会降低一点
					//_self.pixelRatio =res.pixelRatio;
					_self.pixelRatio = 2;
				}
			}
		});
		//#endif
		this.cWidth = uni.upx2px(750);
		this.cHeight = uni.upx2px(500);
		this.cWidth2 = uni.upx2px(700);
		this.cHeight2 = uni.upx2px(1100);
		// this.cWidth3 = uni.upx2px(250);
		// this.cHeight3 = uni.upx2px(250);
		// this.arcbarWidth = uni.upx2px(24);
		// this.gaugeWidth = uni.upx2px(30);

		//this.fillData(Data);
	},
	onReady() {
		this.getServerData();
	},
	methods: {
		getServerData() {
			uni.showLoading({
				title: '正在加载数据...'
			});
			uni.request({
				// url: 'https://unidemo.dcloud.net.cn/hello-uniapp-ucharts-data.json',
				url:'../../static/index.json',
				data: {},
				success: function(res) {
					_self.fillData(res.data);
				},
				fail: () => {
					_self.tips = '网络错误，小程序端请检查合法域名';
				},
				complete() {
					uni.hideLoading();
				}
			});
		},
		fillData(data) {
			this.serverData = data;
			this.tips = data.tips;
			let Pie = {
				series: []
			};
			let Ring = {
				series: []
			};
			//这里我后台返回的是数组，所以用等于，如果您后台返回的是单条数据，需要push进去
			// Column.categories = data.Column.categories;
			// //这里的series数据是后台做好的，如果您的数据没有和前面我注释掉的格式一样，请自行拼接数据
			// Column.series = data.Column.series;
			// ColumnMeter.categories = data.ColumnMeter.categories;
			//这里的series数据,如果为Meter，series[0]定义为外层数据，series[1]定义为内层数据
			Pie.series = data.Pie.series;
			Ring.series = data.Ring.series;
			//自定义文案示例，需设置format字段
			// for (let i = 0; i < Ring.series.length; i++) {
			// 	Ring.series[i].format = () => {
			// 		return Ring.series[i].name + Ring.series[i].data;
			// 	};
			// }
			this.showRing('canvasRing', Ring);
			this.showRing2('canvasRing2', Ring);
			this.showRing('canvasRing3', Ring);
			this.showRing('canvasRing4', Ring);
		},
		showRing(canvasId, chartData) {
			canvasObj[canvasId] = new uCharts({
				$this: _self,
				canvasId: canvasId,
				type: 'ring',
				fontSize: 11,
				padding: [5, 5, 5, 5],
				legend: {
					show: true,
					position: 'right',
					float: 'center',
					itemGap: 10,
					padding: 5,
					lineHeight: 26,
					margin: 5,
					//backgroundColor:'rgba(41,198,90,0.2)',
					//borderColor :'rgba(41,198,90,0.5)',
					borderWidth: 1
				},
				title: {
					name: '70%',
					color: '#7cb5ec',
					fontSize: 25 * _self.pixelRatio
				},
				subtitle: {
					name: '成功率',
					color: '#666666',
					fontSize: 15 * _self.pixelRatio
				},
				extra: {
					pie: {
						lableWidth: 15,
						ringWidth: 40 * _self.pixelRatio, //圆环的宽度
						offsetAngle: 0 //圆环的角度
					}
				},
				background: '#FFFFFF',
				pixelRatio: _self.pixelRatio,
				series: chartData.series,
				animation: false,
				width: _self.cWidth * _self.pixelRatio,
				height: _self.cHeight * _self.pixelRatio,
				disablePieStroke: true,
				dataLabel: true
			});
		},
		showRing2(canvasId, chartData) {
			canvasObj[canvasId] = new uCharts({
				$this: _self,
				canvasId: canvasId,
				type: 'ring',
				fontSize: 11,
				padding: [5, 5, 5, 5],
				legend: {
					show: true,
					position: 'right',
					float: 'center',
					itemGap: 10,
					padding: 5,
					lineHeight: 26,
					margin: 5,
					//backgroundColor:'rgba(41,198,90,0.2)',
					//borderColor :'rgba(41,198,90,0.5)',
					borderWidth: 1
				},
				title: {
					name: '60%',
					color: '#7cb5ec',
					fontSize: 25 * _self.pixelRatio
				},
				subtitle: {
					name: '收益率',
					color: '#666666',
					fontSize: 15 * _self.pixelRatio
				},
				extra: {
					pie: {
						lableWidth: 15,
						ringWidth: 40 * _self.pixelRatio, //圆环的宽度
						offsetAngle: 0 //圆环的角度
					}
				},
				background: '#FFFFFF',
				pixelRatio: _self.pixelRatio,
				series: chartData.series,
				animation: false,
				width: _self.cWidth * _self.pixelRatio,
				height: _self.cHeight * _self.pixelRatio,
				disablePieStroke: true,
				dataLabel: true
			});
		},
		showRing3(canvasId, chartData) {
			canvasObj[canvasId] = new uCharts({
				$this: _self,
				canvasId: canvasId,
				type: 'ring',
				fontSize: 11,
				padding: [5, 5, 5, 5],
				legend: {
					show: true,
					position: 'right',
					float: 'center',
					itemGap: 10,
					padding: 5,
					lineHeight: 26,
					margin: 5,
					//backgroundColor:'rgba(41,198,90,0.2)',
					//borderColor :'rgba(41,198,90,0.5)',
					borderWidth: 1
				},
				title: {
					name: '60%',
					color: '#7cb5ec',
					fontSize: 25 * _self.pixelRatio
				},
				subtitle: {
					name: '收益率',
					color: '#666666',
					fontSize: 15 * _self.pixelRatio
				},
				extra: {
					pie: {
						lableWidth: 15,
						ringWidth: 40 * _self.pixelRatio, //圆环的宽度
						offsetAngle: 0 //圆环的角度
					}
				},
				background: '#FFFFFF',
				pixelRatio: _self.pixelRatio,
				series: chartData.series,
				animation: false,
				width: _self.cWidth * _self.pixelRatio,
				height: _self.cHeight * _self.pixelRatio,
				disablePieStroke: true,
				dataLabel: true
			});
		},
		showRing4(canvasId, chartData) {
			canvasObj[canvasId] = new uCharts({
				$this: _self,
				canvasId: canvasId,
				type: 'ring',
				fontSize: 11,
				padding: [5, 5, 5, 5],
				legend: {
					show: true,
					position: 'right',
					float: 'center',
					itemGap: 10,
					padding: 5,
					lineHeight: 26,
					margin: 5,
					//backgroundColor:'rgba(41,198,90,0.2)',
					//borderColor :'rgba(41,198,90,0.5)',
					borderWidth: 1
				},
				title: {
					name: '60%',
					color: '#7cb5ec',
					fontSize: 25 * _self.pixelRatio
				},
				subtitle: {
					name: '收益率',
					color: '#666666',
					fontSize: 15 * _self.pixelRatio
				},
				extra: {
					pie: {
						lableWidth: 15,
						ringWidth: 40 * _self.pixelRatio, //圆环的宽度
						offsetAngle: 0 //圆环的角度
					}
				},
				background: '#FFFFFF',
				pixelRatio: _self.pixelRatio,
				series: chartData.series,
				animation: false,
				width: _self.cWidth * _self.pixelRatio,
				height: _self.cHeight * _self.pixelRatio,
				disablePieStroke: true,
				dataLabel: true
			});
		},
		
		
		
		touchPie(e, id) {
			canvasObj[id].showToolTip(e, {
				format: function(item) {
					return item.name + ':' + item.data;
				}
			});
		}
	}
};
</script>

<style>
page {
	background: #f2f2f2;
	width: 750upx;
	overflow-x: hidden;
}

.qiun-padding {
	padding: 0.5%;
	width: 96%;
}

.qiun-wrap {
	display: flex;
	flex-wrap: wrap;
}

.qiun-rows {
	display: flex;
	flex-direction: row !important;
}

.qiun-columns {
	display: flex;
	flex-direction: column !important;
}

.qiun-common-mt {
	margin-top: 10upx;
}

.qiun-bg-white {
	background: #ffffff;
}

.qiun-title-bar {
	width: 96%;
	padding: 10upx 2%;
	flex-wrap: nowrap;
}

.qiun-title-dot-light {
	border-left: 10upx solid #0ea391;
	padding-left: 10upx;
	font-size: 32upx;
	color: #000000;
}

/* 通用样式 */
.qiun-charts {
	width: 750upx;
	height: 500upx;
	background-color: #ffffff;
}

.charts {
	width: 750upx;
	height: 500upx;
	background-color: #ffffff;
}

/* 横屏样式 */
.qiun-charts-rotate {
	width: 700upx;
	height: 1100upx;
	background-color: #ffffff;
	padding: 25upx;
}

.charts-rotate {
	width: 700upx;
	height: 1100upx;
	background-color: #ffffff;
}

/* 圆弧进度样式 */
.qiun-charts3 {
	width: 750upx;
	height: 250upx;
	background-color: #ffffff;
	position: relative;
}

.charts3 {
	position: absolute;
	width: 250upx;
	height: 250upx;
	background-color: #ffffff;
}

.qiun-tip {
	display: block;
	width: auto;
	overflow: hidden;
	padding: 15upx;
	height: 30upx;
	line-height: 30upx;
	margin: 10upx;
	background: #ff9933;
	font-size: 30upx;
	border-radius: 8upx;
	justify-content: center;
	text-align: center;
	border: 1px solid #dc7004;
	color: #ffffff;
}
</style>
