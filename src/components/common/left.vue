<template>
    <div class="left">
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
          <i class="icon iconfont icon-gongnengjianyi"></i>
          <div>商品</div>
        </li>
        <li>
          <i class="icon iconfont icon-gongnengjianyi"></i>
          <div>会员</div>
        </li>
        <li>
          <i class="icon iconfont icon-gongnengjianyi"></i>
          <div>统计</div>
        </li>
      </ul>
    </div>
</template>

<script>

    export default {
        name: 'left',
        data() {
            return {
            }
        },
    }
</script>
<style>
  .left{
    width: 5%;
    height: 100%;
    background-color: darkcyan;
    float: left;
    color: white;
    font-size: 10px;
  }
  .left ul{
    margin: 0;
    padding: 0;
  }
li{
  list-style: none;
  text-align: center;
  border-bottom: 1px solid beige;
  padding: 10px;
}
  .iconfont{
    font-size: 24px;
  }

</style>
