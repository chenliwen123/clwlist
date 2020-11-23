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
    listnum:{  // 加载列表友好显示
      handler(){
          this.style()
      },
      deep:true //true 深度监听
    }
  },
  data() {
    return {
      finished: false,  // 是否显示没有更多了
      refreshing: false,// 为true 时是下拉加载中   不用手动值成true,
      ploading:false    // 为了解决 loading 组件内调用夫级传进来的值修改问题
    };
  },
  methods: {
    style(){
      this.$refs.vanlist.$slots.default && this.$refs.vanlist.$slots.default.forEach(element => {
          element.elm.classList.add('list-enter-active')
      })
    },
    onLoad(clear = false) {
        if (this.refreshing) { //  为真的时候 是重新加载数据进来的  
          this.refreshing = false;  // 把下拉刷新值成 false
        }
        if (this.listnum >= this.maxlist && this.listnum != 0) {
          this.finished = true;     // 为true 时显示 “没有更多了”
          this.style();
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
<style>
.TheList .van-list > div{
  opacity: 0;
  transform: translateY(100%);
}
.TheList .list-enter-active {
    transition: .3s;
    opacity: 1 !important;
    transform: translateY(0) !important;
}
.TheList .van-list__finished-text,.TheList .van-list__placeholder,.TheList .van-list__loading{
    opacity: 1 !important;
    transform: translateY(0) !important;
    }
.TheList .list-enter-active:nth-child(10n+1) {
    transition-delay: .1s;
}
.TheList .list-enter-active:nth-child(10n+2) {
    transition-delay: .2s;
}
.TheList .list-enter-active:nth-child(10n+3) {
    transition-delay: .3s;
}
.TheList .list-enter-active:nth-child(10n+4) {
    transition-delay: .4s;
}
.TheList .list-enter-active:nth-child(10n+5) {
    transition-delay: .5s;
}
.TheList .list-enter-active:nth-child(10n+6) {
    transition-delay: .6s;
}
.TheList .list-enter-active:nth-child(10n+7) {
    transition-delay: .7s;
}
.TheList .list-enter-active:nth-child(10n+8) {
    transition-delay: .8s;
}
.TheList .list-enter-active:nth-child(10n+9) {
    transition-delay: .9s;
}
.TheList .list-enter-active:nth-child(10n+10) {
    transition-delay: 1s;
}
</style>
<style scoped>
</style>