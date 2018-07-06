# 使用手册


---

## 1.scroll ##
功能：超出高度后的惯性滚动
最基础的滑动组件，暴露了一些核心功能接口；
**需要安装better-scroll**；
具体参照better-scroll的API文档；

## 2.left-slide ##
基于scroll的左滑删除组件；
功能：
1.支持左右滑动（滑动距离不够的话回自动滑回去）；
2.如果自身的滑块已经滑出来，则点击事件不响应；
3.开始滑动或者点击某项时，其余滑出的滑块都会自动滑回；
4.只有自身的滑块未滑出来，才响应点击事件；
**需要安装better-scroll和vue-click-outside，引入scroll组件；**

    <left-slide :liItem="item" class="item"
                        @emitClick="emitClick"
                        @scrollStart="scrollStart"
                        v-for="(item,index) in discList" :key="index" :index="index" ref="leftSlide">
              <div class="visible" slot="visible">
              <div class="del" slot="invisible" @click.stop="del(item)">删除</div>
            </left-slide>
每个left-slide组件相当于一个ul中的li；
其中一个visible插槽为当前可见的撑满整个横屏的div，需为行内元素才能在一排显示；
另一个invisible插槽为左滑出来的一个div（目前宽度写死为100px)，需为行内元素才能在一行显示;
需要给子组件传对象：liItem=“item”以及当前left-slide的唯一索引值；
接收一个子组件的点击事件emitClick，参数为对象{data，flag}，data为之前传入的对象，flag为当前当前点击事件是否应该相应（即当前item的隐藏的滑块滑出来的时候，默认点击事件不响应，而是把滑块滑回去）；
接收一个子组件的滑动开始监听事件，参数为之前传入的index索引值，也就是说在监听到某个left-slide组件开始滑动时，其他的需要都滑回到最初始位置，left-slide组件提供了scrollToOrigin方法；
**滑块的点击事件一定要阻止冒泡；**

## 3.联系人列表 ##
基于scroll组件；
**需要安装better-scroll库，引入scroll组件**；
功能：
1.联系人按首字母排序；
2.整个列表支持纵向的惯性滚动；
3.右侧有首字母索引列表，点击首字母，左侧联系人列表可立即跳至对应段，滑动首字母索引，左侧联系人也可跟随滑动；
4.左侧联系人列表中的首字母提示项，如果页面可视区域在该字母段内，会一直保持在顶部，直到被下一个顶掉2；

    <list-view :data="singers" @select="selectSinger" ref="list"></list-view>

接收一个参数，即联系人数组数据；
数组中对象结构必须为{title：‘首字母’，items：属于该首字母的人员数组}；
接收一个点击事件，参数为点击项的人员对象；

## 4.加载中 ##
功能：提示正在加载中

    <loading :title='正在加载中'></loading>
    
支持传入提示字

## 5.确认框 ##
功能：出现提示框，并带有取消，确定按钮，全屏附有蒙层

    <confirm ref="confirm" title="是否清空所有搜索历史" cancelBtnText='取消' confirmBtnText="清空" @confirm="confirm" @cancel='cancel'></confirm>
    
可以自定义提示内容和底部两个按钮内容并接收两个按钮的点击事件

## 6.轮播图 ##
基于better-scroll；
**需要安装better-scroll库；**
功能：支持图片的自动循环轮播；
自定义参数有loop：是否循环轮播；autoPlay：是否自动轮播；interval：自动轮播间隔时间（ms单位）；

## 7.进度条 ##
功能：
1.根据传入参数，显示进度；
2.支持左右拖动和点击，显示进度；

    <progress-bar :percent="percent" @percentChange="onProgressBarChange"></progress-bar>
    
支持传入百分比参数来控制进度条（percent <= 1）;
接收进度条改变事件的监听，参数为percent；







