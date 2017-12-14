<template>
  <div>
    <t-search></t-search>
    <div class="list-header">
      <ul class="nav" :class="[current]">
        <li class="sales" @click="isActive('sales')" type="a">销量</li>
        <li class="price" @click="isActive('price')" type="b">价格</li>
        <li class="brand" @click="isActive('brand')" type="c">品牌</li>
      </ul>
    </div>
    <div style="font-size: 20px;">当前页面是{{current}}</div>
    <div class="list-content">
      <ul>
        <li v-for="item in productList">
          <div class="left-icon">
            <img :src="item.imgUrl">
          </div>
          <div class="right-content">
            <div class="productname">{{item.name}}</div>
            <span class="productprice">${{item.price}}</span>
            <span class="productnum">销量：{{item.buyCount}}</span>
          </div>
        </li>
      </ul>
    </div>

  </div>
</template>
<script>
   import tSearch from "@/components/category/search";
   import api from "@/api";
  export default {
    components:{tSearch},
    data(){
      return{
        current:'sales',//默认销量页
        productList:[],
        postData:{
          requrl:api.goods.goodlistbycatid,
          catId:'',
          brandIds:'',

          sortName:'price',//buyCount,
          sortOrder:'',
          pageIndex:'',
          pageSize:''
        }
      }
    },
    mounted() {
      var vm = this;
      vm.postData.catId = vm.$route.query.catid;
      vm.getCate();
      var allData = data.data.data.list

    },
    methods: {
      isActive (cur) {
        console.log(cur);
        this.current = cur;//当前选项

        console.log(allData);

      },
      getCate() {
        var that = this;
        that.$axios.post(api.post,that.postData).then(data => {
          if(data.data.code=='999'){
            that.productList=data.data.data.list;
          }
        }).catch(err=>{

        });
      }
    }
  }
</script>
<style scoped lang="scss">
  .list-header{
    width: 100%;
    ul{
      display: flex;
      padding-top:46px;
      li{
        width: 33.3%;
        height: 36px;
        text-align: center;
        line-height: 36px;
        background: #fff;
        color: #666666;
      }
    }
  }
  .list-content{

    li{
      height: 100px;
      background: #fff;
      border-top:1px solid #E8E8E8;;
      padding:8px 16px;
      display: flex;
      .left-icon{
        img{
          width: 100px;
          height: 100px;
          -webkit-background-size: cover;
          background-size: cover;
        }
      }
      .right-content{
        width: 227px;
        margin:8px 0 0 8px;
        position: relative;
        .productname{
          font-size: 14px;
          color: #333333;
          line-height: 20px;
          display: -webkit-box;
          -webkit-box-orient: vertical;
          -webkit-line-clamp: 2;
          overflow: hidden;
        }
        .productprice{
          position: absolute;
          left:0;
          bottom:0;
          font-size: 16px;
          color: #FF5000;
        }
        .productnum{
          position: absolute;
          right:0;
          bottom:0;
          font-size: 12px;
          color: #999999;
        }
      }
    }
  }
  .sales .sales, .price .price, .brand .brand{
    color: #42BD56 ;
    font-weight: bold;
  }
</style>
