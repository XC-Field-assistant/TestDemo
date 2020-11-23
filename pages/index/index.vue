<template>
	<view>
		
		<navigator url="../my/my"><image src="../../static/我的.png" id="my" @click="goto"></image></navigator>
		
		<view id="my_search">
			<view class="row">
				<icon type='search'/>
				<input class="input" placeholder='输入地址' v-model="clientname" @input='inputes' focus auto-focus></input>
			</view>
		</view>
		
		<image src="../../static/主页地图.jpg" class="index_photo"></image>
		
	</view>
	
	
	
</template>

<script>
	export default {
		
		data() {
			return {
				list:[],
				clientname:'',
				location:{}
			}
		},
		onLoad() {
			uni.getLocation({
				type:'gcj02',
				altitude:true,
				geocode:true,
				success: (res) => {
					this.location = res;
				}
			})
		},
		methods: {
			inputes(e){
				this.getLocationList(e.detail.value);
			},
			getLocationList(keywords) {
				let _this = this;
				let data = {};
				data.keywords = keywords;
				data.location = this.location.longitude+','+this.location.latitude;
				data.key="196e78135f0bc48a751225a336646ecb";
				uni.request({
					url: 'https://restapi.amap.com/v3/assistant/inputtips?parameters',
					method: 'GET',
					data: data,
					success(res) {
						_this.list = res.data.tips;
					}
				})
			},
			selectLocation(item){
				console.log(item);
				this.clientname = item.name;
				this.list = [];
			},
			goto(){
				this.$router.push('/pages/my/my');
			}
		}
		
	}
</script>

<style>
#my{
	height: 50px;
	width: 50px;
	float: left;
	position: absolute;
	margin-left: 265px;
	margin-top: 3px;
	z-index: 3;
}
#my_search{
	width: 250px;
	height: 46px;
	float: left;
	position: absolute;
	margin-top: 5px;
	margin-left: 5px;
	border-style: solid;
	border-width: 1px;
	z-index: 2;
}
.row{
		display: flex;
		align-items: center;
		position: relative;
		padding:0 30upx;
		height: 110upx;
		background: #ffffff;
	}
	.input{
		width: 250px;
		height: 50px;
		padding-left: 10px;
	}
.index_photo{
	z-index: 1;
	height: 470px;
	width: 325px;
	
	position:inherit;
}
	
</style>
