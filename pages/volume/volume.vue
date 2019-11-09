<template>
	<div class="card__body">
		<uni-card title="今日计划交易额" thumbnail="/static/qiandai.png">
			<view class="uni-card__content-extra" mode="extra">500.00.00元</view>
		</uni-card>
		<uni-card title="昨日计划交易额" thumbnail="/static/qiandai.png"><view class="uni-card__content-extra" mode="extra">500.00.00元</view></uni-card>
		<uni-card title="本周交易额" thumbnail="/static/qiandai.png"><view class="uni-card__content-extra" mode="extra">500.00.00元</view></uni-card>
		<uni-card title="本月交易额" thumbnail="/static/qiandai.png"><view class="uni-card__content-extra" mode="extra">500.00.00元</view></uni-card>
	</div>
</template>

<script>
import uniCard from '@/components/uni-card/uni-card';
export default {
	components: { uniCard }
};
</script>

<style lang="less">
.card__body{
	.uni-card{
		margin: 20rpx 12px;
		.uni-card__content{
			.uni-card__content-extra{
				text-align: center;
				font-size: 80rpx;
				color: #F00;
			}
		}
	}
}
</style>