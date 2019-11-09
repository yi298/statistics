<template>
    <view>
        <view class="page-body">
			<view class="btn-area">
			    <navigator url="/pages/login/login" open-type="navigate" hover-class="navigator-hover">
			        <button type="default">hi~您未登录</button>
			    </navigator>
			</view>
            <view class="btn-area">
                <navigator url="/pages/login/reg" open-type="navigate" hover-class="navigator-hover">
                    <button type="default">注册账号</button>
                </navigator>
                <navigator url="/pages/login/pwd" open-type="navigate" hover-class="navigator-hover">
                    <button type="default">找回密码</button>
                </navigator>
				<navigator url="/pages/login/login" open-type="navigate" hover-class="navigator-hover">
                    <button type="default">关于</button>
                </navigator>
            </view>
        </view>
    </view>
</template>

<script>
	export default {
	    onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
	        console.log(option.id); //打印出上个页面传递的参数。
	        console.log(option.name); //打印出上个页面传递的参数。
	    }
	}
</script>

<style>
</style>
