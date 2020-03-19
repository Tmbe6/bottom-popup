<template>
	<view class="popupList">
		<view class="cu-bar bg-white select-title">
			<view class="action text-blue"></view>
			<text>{{popupTitle.topTitle}}</text>
			<view class="action text-black" @click="hideModal">
				<text class="cuIcon-close"></text>
			</view>
		</view>
		<view class="select-list">
			<view class="select-input" @click="selectFirst">
				<input type="text" :placeholder="popupTitle.firstTitle" v-model="firstvalue" disabled>
				<text :class="[isSelectfirst == false ? 'cuIcon-right' : 'cuIcon-unfold']"></text>
			</view>
			<scroll-view scroll-y="true" class="select-list-content" v-if="isSelectfirst">
				<view class="select-type" v-for="(item,index) in firstList" :key="item.id" @click="clickFirst(item.id)">{{item.name}}</view>
			</scroll-view>
			<scroll-view scroll-x="true" style="width: 100%;">
				<view class="input-subject" @click="selectSecond" v-if="firstvalue != ''">
					<view class="checked-list">
						<input type="text" :placeholder="popupTitle.secondTitle" disabled v-if="checkedList.length == 0">
						<view class="checked-subject" v-for="(item,index) in checkedList" :key="index" v-else @click.stop="delSecond(index,item.id)">
							<text>{{item.name}}</text><text class="cuIcon-close"></text>
						</view>
					</view>
					<text :class="[isSelectsecond == false ? 'cuIcon-right' : 'cuIcon-unfold','subject-text']"></text>
				</view>
			</scroll-view>
			<scroll-view scroll-y="true" class="select-list-content" v-if="isSelectsecond">
				<view class="select-subject" v-for="(item,index) in secondList" :key="item.id">
					<text>{{item.name}}</text>
					<radio class='blue' :class="item.checked == true ? 'checked' : ''" :checked="item.checked" @click="checkChange(item.id)"></radio>
				</view>
			</scroll-view>
			<view class="btn" @click="submit">
				<button class="bg-blue">确定</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:{
			popupTitle: {
				type: Object,
				default (){
					return {
						topTitle: '请选择',
						firstTitle: '请选择一级项目',
						secondTitle: '请选择二级项目'
					}
				}
			},
			// 第一级下拉选择菜单
			firstLists: {
				type: Array,
				default (){
					return []
				}
			},
			// 第二级下拉选择菜单
			secondLists: {
				type: Array,
				default (){
					return []
				}
			}
		},
		data() {
			return {
				showPopup: true,	//是否底部弹窗
				isSelectfirst: false,	//是否选择一级菜单
				isSelectsecond: false,   //是否选择二级菜单
				firstvalue: '',		//一级菜单选中的值
				checkedList: [],   //二级菜单选中的值
				firstList: [],
				secondList: []
			};
		},
		created() {
			this.firstList = this.firstLists;
			this.secondList = this.secondLists;
		},
		methods:{
			// 显示底部弹窗
			showModal(e) {
				this.showPopup = true;
			},
			// 隐藏底部弹窗
			hideModal() {
				this.showPopup = false;
				this.secondList.forEach((item) => {
					item.checked = false;
				})
				this.$emit('isShowPopup',this.showPopup);
			},
			// 是否选择一级菜单
			selectFirst(){
				this.isSelectfirst = !this.isSelectfirst;
			},
			// 是否选择二级菜单
			selectSecond(){
				this.isSelectsecond = !this.isSelectsecond;
			},
			// 点击选择
			clickFirst(id){
				this.firstvalue = this.firstList[id].name;
				this.isSelectfirst = false;
			},
			// 选中二级菜单按钮触发
			checkChange(id){
				if(this.secondList[id].checked == false){
					this.checkedList.push({id: id,name: this.secondList[id].name});
					this.secondList[id].checked = true;
				}else{
					this.checkedList.forEach((val,index) => {
						if(val.id == id){
							this.checkedList.splice(index,1);
						}
					})
					this.secondList[id].checked = false;
				}
			},
			// 删除选择的二级菜单选项
			delSecond(index,id){
				this.secondList[id].checked = false;
				this.checkedList.splice(index,1);
			},
			// 确定选择
			submit(){
				this.isSelectfirst = false;
				this.isSelectsecond = false;
				this.$emit('selectInfo',{
					firstVal: this.firstvalue,
					secondVal: this.checkedList
				})
				this.secondList.forEach((item) => {
					item.checked = false;
				})
				this.hideModal();
			}
		}
	}
</script>

<style lang="scss" scoped>
	.popupList{
		width: 100%;
		.select-title{
			font-size: 30rpx;
		}
		.select-list{
			width: 100%;
			min-height: 300rpx;
			background-color: #fff;
			.select-input{
				width: 100%;
				height: 80rpx;
				border-top: 1rpx solid #f1f1f1;
				border-bottom: 1rpx solid #f1f1f1;
				display: flex;
				flex-direction: row;
				align-items: center;
				justify-content: space-between;
				text-align: left;
				input{
					height: 100%;
					// width: 500rpx;
					font-size: 28rpx;
					padding-left: 40rpx;
					color: #0081FF;
				}
				text{
					padding-right: 40rpx;
				}
			}
			.input-subject{
				width: 100%;
				height: 80rpx;
				border-top: 1rpx solid #f1f1f1;
				border-bottom: 1rpx solid #f1f1f1;
				display: flex;
				flex-direction: row;
				align-items: center;
				justify-content: space-between;
				text-align: left;
				.checked-list{
					// width: 700rpx;
					display: flex;
					flex-direction: row;
					font-size: 20rpx;
					color: #0081FF;
					input{
						padding-left: 40rpx;
						font-size: 22rpx;
					}
					.checked-subject{
						display: flex;
						align-items: center;
						justify-content: center;
						border-radius: 10rpx;
						padding-left: 15rpx;
						padding-right: 15rpx;
						white-space: nowrap;
						height: 40rpx;
						margin-left: 5rpx;
						background-color: rgb(222,244,253);
						&:first-child{
							margin-left: 30rpx;
						}
						&:last-child{
							margin-right: 30rpx;
						}
						text{
							&:last-child{
								margin-left: 5rpx;
							}
						}
					}
				}
				.subject-text{
					padding-right: 40rpx;
				}
			}
			.select-list-content{
				width: 100%;
				height: 300rpx;
				background-color: red;
				font-size: 28rpx;
				.select-type{
					width: 100%;
					height: 79rpx;
					background-color: #fff;
					border-top: .5rpx solid #f1f1f1;
					border-bottom: .5rpx solid #f1f1f1;
					display: flex;
					align-items: center;
					justify-content: center;
					&:first-child{
						border-bottom: 0;
					}
					&:active{
						color: #0081FF;
					}
				}
				.select-subject{
					width: 100%;
					height: 79rpx;
					background-color: #fff;
					border-top: .5rpx solid #f1f1f1;
					border-bottom: .5rpx solid #f1f1f1;
					display: flex;
					align-items: center;
					justify-content: center;
					text{
						font-size: 25rpx;
					}
					radio{
						margin-left: 20rpx;
						transform:scale(0.7);
					}
				}
			}
			.btn{
				width: 420rpx;
				margin: 50rpx auto;
			}
		}
	}
</style>
