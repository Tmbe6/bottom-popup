<template>
	<view class="content">
		<button class="bg-blue" @click="clickBtn">底部二级多选下拉菜单</button>
		<view class="cu-modal bottom-modal" :class="showPopup==true?'show':''">
			<view class="cu-dialog">
				<bottom-popup v-if="showPopup" :firstLists="firstList" :secondLists="secondList" :popupTitle="popupTitle" @selectInfo="selectInfo" @isShowPopup="isShowPopup"></bottom-popup>
			</view>
		</view>
	</view>
</template>

<script>
	import bottomPopup from '@/components/bottom-popup/bottom-popup.vue'
	export default {
		components: {bottomPopup},
		data() {
			return {
				showPopup: false,
				popupTitle: {
					topTitle: '请选择',
					firstTitle: '请选择一级项目',
					secondTitle: '请选择二级项目'
				},
				firstList: [{id:0,name:'一级选项一'},{id:1,name:'一级选项二'},{id:2,name:'一级选项三'},{id:3,name:'一级选项四'}],
				secondList: [{id:0,name:'二级选项一',checked: false},{id:1,name:'二级选项二',checked: false},{id:2,name:'二级选项三',checked: false},{id:3,name:'二级选项四',checked: false},{id:4,name:'二级选项五',checked: false},{id:5,name:'二级选项六',checked: false}]
			}
		},
		onLoad() {
			
		},
		methods: {
			// 点击底部弹窗
			clickBtn(){
				this.showPopup = true;
			},
			// 接收子组件传递来的值（底部弹窗）
			selectInfo(e){
				this.showPopup = false;
				console.log(e);
			},
			// 子组件关闭底部弹窗传递的值
			isShowPopup(e){
				this.showPopup = e;
			}
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		
	}
</style>
