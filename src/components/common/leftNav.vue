<template>
    <div class="left_nav">
      <ul>
        <li>
          <i class="icon iconfont icon-goumai"></i>
          <div>收银</div>
        </li>
        <li>
          <i class="icon iconfont icon-dianpu"></i>
          <div>店铺</div>
        </li>
        <li>
          <i class="icon iconfont icon-hanbao"></i>
          <div>商品</div>
        </li>
        <li>
          <i class="icon iconfont icon-huiyuanqia"></i>
          <div>会员</div>
        </li>
        <li>
          <i class="icon iconfont icon-tongji"></i>
          <div>统计</div>
        </li>
        <li>
          <i class="icon iconfont icon-gongnengjianyi"></i>
          <div>设置</div>
        </li>
      </ul>
    </div>
</template>

<script>
    export default {
        name: "left-nav"
    }
</script>

<style scoped>
  .left_nav{
    height:100%;
    width: 56px;
    font-size: 10px;
    font-family: "Microsoft JhengHei";
    color: #fff;
    background-color: #1D8CE0;
    float: left;
  }
  .icon{
    font-size: 25px;
  }
  .left_nav ul{
    padding: 0;
    margin: 0;
  }
  .left_nav li{
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #20A0FF;
    padding: 10px;
  }
</style>
