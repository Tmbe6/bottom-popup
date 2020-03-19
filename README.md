# 前言
bottom-popup 是一个底部弹窗下拉二级菜单（二级可多选）
# 开始
## 使用
<popup-list v-if="showPopup" :firstLists="typeList" :secondLists="subjectList" :popupTitle="popupTitle" @selectInfo="selectInfo" @isShowPopup="isShowPopup"></popup-list>

firstLists 为需要选择的一级菜单数据                                            
secondLists 为需要选择的二级菜单数据                                                                                              
popupTitle 为底部弹窗的标题、一级菜单默认提示、二级菜单默认提示                                                

## 子组件接收
### 底部弹窗的标题、一级菜单默认提示、二级菜单默认提示                                                                                     
popupTitle: {type: Object,default(){return {topTitle: '请选择',firstTitle: '请选择一级项目',secondTitle: '请选择二级项目'}}}                                         
### 第一级下拉选择菜单                                                                                                                
firstLists: {type: Array,default(){return []}}                                                                                        
                                                                                                      
### 第二级下拉选择菜单                                                                                                
secondLists: {type: Array,default(){return []}}                                                                                                                 


