<template>
  <div class="clearfix selector">
    <!-- 品牌logo信息 -->
    <div class="type-wrap logo">
      <div class="fl key brand">品牌</div>
      <div class="value logos">
        <ul class="logo-list">
          <li
            v-for="trademark in trademarkList"
            :key="trademark.brandId"
            @click="tradeMarkHandler(trademark)"
          >
          <img  :src="trademark.brandImg"/>

          </li>
        </ul>
      </div>
      <div class="ext">
        <a href="javascript:void(0);" class="sui-btn">多选</a>
        <a href="javascript:void(0);">更多</a>
      </div>
    </div>
    <!-- 品牌售卖属性 -->
    <div class="type-wrap" v-for="(attr, index) in attrsList" :key="index">
      <div class="fl key">{{ attr.attrName }}</div>
      <div class="fl value">
        <ul class="type-list">
          <li
            v-for="(attrValue, index) in attr.attrValue"
            :key="index"
          >
            <a @click="attrInfoHandler(attr.attrId, attrValue)"><el-tag type="info">{{ attrValue }}</el-tag></a>
          </li>
        </ul>
      </div>
      <div class="fl ext"></div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  name: "SearchSelector",
  computed: {
    ...mapGetters(["attrsList", "trademarkList"]),
  },
  data() {
      return {
        attrs: []
      }
  },
  methods: {
    // 我终于理解了！前端请求结果和Postman请求结果不一样是因为Postman没有库存限制！！！前端请求设置库存为true
    // 品牌事件处理函数
    tradeMarkHandler(trademark) {
      // 需要整理参数。向服务器发送请求 ==> 需要在父组件发送请求
      // 因为父组件中params参数是带给服务器的参数
      // 使用自定义事件
      this.$emit("trademarkInfo", trademark);
    },
    // 平台售卖属性信息
    attrInfoHandler(attrId, attrValue) {
      // attrs=1_白色:蓝色&attrs=2_2寸:5寸【属性可多选，值也可多选】
      let attrInfo = attrId+'_' + attrValue;
      this.$emit('attrInfo', attrInfo)
    },
  },
};
</script>

<style lang="less" scoped>
.selector {
  border: 1px solid #ddd;
  margin-bottom: 5px;
  overflow: hidden;

  .logo {
    border-top: 0;
    margin: 0;
    position: relative;
    overflow: hidden;

    .key {
      padding-bottom: 87px !important;
    }
  }

  .type-wrap {
    margin: 0;
    position: relative;
    border-top: 1px solid #ddd;
    overflow: hidden;

    .key {
      width: 100px;
      background: #f1f1f1;
      line-height: 26px;
      text-align: right;
      padding: 10px 10px 0 15px;
      float: left;
    }

    .value {
      overflow: hidden;
      padding: 10px 0 0 15px;
      color: #333;
      margin-left: 120px;
      padding-right: 90px;

      .logo-list {
        li {
          float: left;
          border: 1px solid #e4e4e4;
          margin: -1px -1px 0 0;
          width: 105px;
          height: 52px;
          text-align: center;
          line-height: 52px;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          font-weight: 700;
          color: #e1251b;
          font-style: italic;
          font-size: 14px;

          img {
            max-width: 100%;
            vertical-align: middle;
          }
        }
      }

      .type-list {
        li {
          float: left;
          display: block;
          margin-right: 30px;
          line-height: 26px;

          a {
            text-decoration: none;
            color: #666;
          }
        }
      }
    }

    .ext {
      position: absolute;
      top: 10px;
      right: 10px;

      .sui-btn {
        display: inline-block;
        padding: 2px 14px;
        box-sizing: border-box;
        margin-bottom: 0;
        font-size: 12px;
        line-height: 18px;
        text-align: center;
        vertical-align: middle;
        cursor: pointer;
        padding: 0 10px;
        background: #fff;
        border: 1px solid #d5d5d5;
      }

      a {
        color: #666;
      }
    }
  }
}
</style>
