<template>
  <div class="cart-one-item">
    <!--左侧-->
    <div class="left">
      <img :src="imgUrl">
    </div>
    <!--右侧-->
    <div class="right">
      <!--商品描述-->
      <div class="text">
        <h3 class="title">{{title}}</h3>
        <p class="content">{{content}}</p>
        <div>
          <span class="price"> ￥
            <span class="price-number">{{price}}</span>
          </span>
        </div>
      </div>
      <!--购物车按钮-->
      <div class="cart-btn">
        <!--操作购物车加减组件-->
        <my-input-number :count="counter" @changeNumberEvent="getOperator"></my-input-number>
      </div>
    </div>
    <!--删除商品对话框-->
    <div class="dialog-box" @click.stop="">
      <el-dialog :visible.sync="dialogVisible" width="80%" :show-close="false">
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
          <el-button type="danger" @click="confirmDelete">删 除</el-button>
        </span>
      </el-dialog>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    name: 'cartOneItem',
    props: [
      'itemId',
      'imgUrl',
      'title',
      'content',
      'price',
      'count'
    ],
    data () {
      return {
        dialogVisible: false
      }
    },
    computed: {
      id () {
        return this.itemId
      },
      counter () {
        let that = this
        let cartGoods = this.$store.state.cartGoods
        let result = 0
        cartGoods.some(good => {
          if (good.id === that.id) {
            result = good.count
          }
        })
        return result
      }
    },
    methods: {
      // 操作数量
      getOperator (op) {
        if (op === 'plus') {
          this.$store.commit('addGoods', this.id)
        } else {
          if (this.counter === 1) {
            this.dialogVisible = true
          } else {
            this.$store.commit('reduceGoods', this.id)
          }
        }
      },
      // 确认删除
      confirmDelete () {
        this.dialogVisible = false
        this.$store.commit('deleteGoodsFormCart', this.id)
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import "../assets/css/variable";

  .cart-one-item{
    height: $OneCommodityWrapHeight; /* 130px */
    width: 100%;
    font-size: 0;
    border-bottom: 1px solid #e4e7ed;
    background: #fff;
  }

  .cart-one-item > div {
    font-size: $GobalFontSize; /* 14px */
    display: inline-block;
    vertical-align: middle;
  }

  .left {
    width: 40%;
    height: 100%;
  }

  .left > img {
    height: 80%;
    transform: translateY(10%);
  }

  .right {
    box-sizing:  border-box;
    position: relative;
    height: 100%;
    width: 60%;
    text-align: left;
    padding-left: 10px;

    .text {
      position: absolute;
      line-height: 30px;
      top: 50%;
      transform: translateY(-50%);
    }

    .title {

    }

    .price {

      .price-number {
        color: #ff65af;
        font-size: 18px;
        font-weight: 600;
      }
    }

    .cart-btn {
      position: absolute;
      bottom: 5px;
      right: 10px;

      .el-button-danger {
        background-color: #ff65af;
        border-color: #ff65af;
      }
    }
  }

  @media screen and (min-width: 600px){
    .cart-one-item {
      height: $OneCommodityWrapHeight600;
    }
  }

  @media screen and (min-width: 800px) {
    .cart-one-item {
      height: $OneCommodityWrapHeight800;
    }
  }

  @media screen and (min-width: 1025px){
    .cart-one-item {
      height: $OneCommodityWrapHeight1025;
    }
  }
</style>
