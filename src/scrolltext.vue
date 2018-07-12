<template>
  <div id="app">
    <ul class="tab">
      scroll头部
    </ul>
    <div class="position-box">
        <vue-better-scroll
            class="wrapper"
            ref="scroll"
            :scrollbar="scrollbarObj"
            :pullDownRefresh="pullDownRefreshObj"
            :pullUpLoad="pullUpLoadObj"
            @pullingDown="onPullingDown"
            @pullingUp="onPullingUp">
          <ul ref="list" class="list-content" style="min-height: 95vh">
            <li>tab2</li>
            <li v-for="item in items1">{{item}}</li>
          </ul>
        </vue-better-scroll>
    </div>
  </div>
</template>

<script>
  import VueBetterScroll from './lib'

  let count = 1
  export default {
    name: 'app',
    components: { VueBetterScroll },
    data () {
      return {
        toggle:1,
        // 这个配置可以开启滚动条，默认为 false。当设置为 true 或者是一个 Object 的时候，都会开启滚动条，默认是会 fade 的
        scrollbarObj: {
          fade: true
        },
        // 这个配置用于做下拉刷新功能，默认为 false。当设置为 true 或者是一个 Object 的时候，可以开启下拉刷新，可以配置顶部下拉的距离（threshold） 来决定刷新时机以及回弹停留的距离（stop）
        pullDownRefreshObj: {
          threshold: 50,
          stop: 40
        },
        // 这个配置用于做上拉加载功能，默认为 false。当设置为 true 或者是一个 Object 的时候，可以开启上拉加载，可以配置离底部距离阈值（threshold）来决定开始加载的时机
        pullUpLoadObj: {
          threshold: 0,
          txt: {
            more: '加载更多',
            noMore: '没有更多数据了'
          }
        },
        items1: [],
        items2: []
      }
    },
    mounted () {

    },
    created () {
      this.onPullingDown();
    },
    methods: {
      // 模拟数据请求
      getData () {
        return new Promise(resolve => {
          setTimeout(() => {
            const arr = []
            for (let i = 0; i < 20; i++) {
              arr.push(count++)
            }
            resolve(arr)
          }, 1000)
        })
      },
      onPullingDown () {
        // 模拟下拉刷新
        console.log('下拉刷新1')
        count = 0
        this.getData().then(res => {
          this.items1 = res
          this.$refs.scroll.forceUpdate(true)
        })
      },
      onPullingUp () {
        // 模拟上拉 加载更多数据
        console.log('上拉加载1')
        this.getData().then(res => {
          this.items1 = this.items1.concat(res)
          if(count<50){
            this.$refs.scroll.forceUpdate(true)
          }else{
            this.$refs.scroll.forceUpdate(false)
          }
        })
      }
    }
  }
</script>

<style>
  * {
    padding: 0;
    margin: 0;
  }

  body, html {
    height: 100%;
    overflow: hidden;
  }

  #app {
    font-family: '微软雅黑', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    font-size: 18px;
  }
  ul.tab{
    position: fixed;
    top: 0px;
    right: 0;
    left: 0;
    height: 40px;
    z-index: 9;
    background-color: yellowgreen;
    display: flex;
    list-style: none;
    border-bottom: 1px solid #000;
  }
  ul.tab li{
    border-right: 1px solid #000;
    width: 50%;
  }
  .position-box{
    position: fixed;
    top: 40px;
    right: 0;
    left: 0;
    bottom: 0;
  }

  .wrapper{
    height: 100%;
  }
  .list-content {
    list-style: none;
    background: #fff;
  }
  .list-content li {
    height: 40px;
  }
  .list-item {
    height: 60px;
    line-height: 60px;
    font-size: 18px;
    padding-left: 20px;
    border-bottom: 1px solid #e5e5e5;
  }
  .go-top{
    position: fixed;
    right: 20px;
    bottom: 20px;
    background-color: #009a61;
    border-radius: 5px;
    border: 1px solid #fff;
    color: #fff;
    padding: 10px 15px;
  }
</style>
