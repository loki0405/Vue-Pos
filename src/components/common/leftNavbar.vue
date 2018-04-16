<template>
	<div class="let-nav">
		<ul>
			<li class="active">
			<i class="iconfont icon icon-canting"></i>
			<div>收银</div>
			</li>
			<li>
			<i class="iconfont icon icon-zhusu"></i>
			<div>餐厅</div>
			</li>
			<li>
			<i class="iconfont icon icon-canyin"></i>
			<div>商品</div>
			</li>
			<li>
			<i class="iconfont icon icon-shezhi"></i>
			<div>设置</div>
			</li>
			<li>
			<i class="iconfont icon icon-geren"></i>
			<div>会员</div>
			</li>
		</ul>
	</div>
</template>

<script>
export default{
		name:'pos'
	}
</script>

<style>
.let-nav{
	height: 100%;
	width: 5%;
	color: #FFFFFF;
	background-color: #128bb4;
	font-size: 14px;
	float: left;
}
.iconfont{
	font-size: 24px;
}
.let-nav ul{
	margin: 0;
	padding: 0;
}
.let-nav ul li{
	list-style: none;
	text-align: center;
	border-bottom: 1px solid white;
	padding: 10px;
	cursor: pointer;
}
.active{
	background-color: lightskyblue;
}
</style>