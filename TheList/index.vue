<template>
<div class="TheList">
  <van-pull-refresh v-model="refreshing" @refresh="onRefresh">
    <van-list
      v-model="ploading"
      :finished="finished"
      ref="vanlist"
      :finished-text="finishedtext"
      @load="onLoad">
      <slot/>
    </van-list>
  </van-pull-refresh>
</div>
</template>

<script>
export default {
  name:"TheList",
  props:{  // 没有数据提示词
    finishedtext:{
      type:String,
      default:'没有更多了'
    },
    listnum:{  // 当前列表数
      type:Number,
      default:0
    },
    maxlist:{ // 列表的总数
      type:Number,  
      default:0
    },
    loading:{ // 为true 显示加载中 false 隐藏
      type:Boolean,
      default:false
    }
  },
  components:{
    
  },
  watch:{
    loading:{  // 解决loading 警告
      handler(){
        this.ploading = this.loading;
      },  
      deep:true //true 深度监听
    },
  },
  data() {
    return {
      finished: false,  // 是否显示没有更多了
      refreshing: false,// 为true 时是下拉加载中   不用手动值成true,
      ploading:false    // 为了解决 loading 组件内调用夫级传进来的值修改问题
    };
  },
  methods: {
    onLoad(clear = false) {
        if (this.refreshing) { //  为真的时候 是重新加载数据进来的  
          this.refreshing = false;  // 把下拉刷新值成 false
        }
        if (this.listnum >= this.maxlist && this.listnum != 0) {
          this.finished = true;     // 为true 时显示 “没有更多了”
          return
        }
      this.$emit('onLoad',clear)
    },
    onRefresh() {
      this.finished = false;   // 下拉刷新后  隐藏 “没有更多了”
      this.refreshing = true;  // 下拉刷新 显示 “加载中”
      setTimeout(() => {
        this.onLoad({clear:true});         // 下拉刷新后  调用加载数据方法
      }, 1000);
    },
  },
  mounted(){
  }
};
</script>