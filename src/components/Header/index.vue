<template>
  <!-- 头部 -->
  <header class="header">
    <!-- 头部的第一行 -->
    <div class="top">
      <div class="container">
        <div class="loginList">
          <p>小济智家欢迎您！</p>
          <p v-if="!logon">
            <span>请</span>
            <router-link to="/login">登录</router-link>
            <router-link class="register" to="/register">免费注册</router-link>
          </p>
          <p v-else>
           <span>你好，{{ userInfo.nickname}}</span>
            <a class="register" @click="logout">退出登录</a>
          </p>
        </div>
        <div class="typeList">
          <router-link to="/center/myorder">我的订单</router-link>
          <router-link to="/shopcart">我的购物车</router-link>
          <router-link to="/###">我的小济智家</router-link>
          <router-link to="/###">小济智家会员</router-link>
          <router-link to="/###">企业采购</router-link>
          <router-link to="/###">关注小济智家</router-link>
          <router-link to="/###">合作招商</router-link>
          <router-link to="/###">商家后台</router-link>
        </div>
      </div>
    </div>
    <!--头部第二行 搜索区域-->
    <div class="bottom">
      <h1 class="logoArea">
        <router-link class="logo" title="小济智家" to="/home">
          <img src="./images/logo.png" alt="" />
        </router-link>
      </h1>
      <div class="searchArea">
        <form action="###" class="searchForm">
          <input
            type="text"
            id="autocomplete"
            class="input-error input-xxlarge"
            v-model="keyword"
            @keyup.enter="goSearch"
          />
          <button
            class="sui-btn btn-xlarge btn-danger"
            type="button"
            @click="goSearch"
          >
            搜索
          </button>
        </form>
      </div>
    </div>
  </header>
</template>

<script>
import {mapGetters} from "vuex";

export default {
  name: "Header",
  data() {
    return {
      keyword: "", // 搜索关键词
    };
  },
  computed: {
    userInfo() {
      return this.$store.state.user.userInfo
    },
    logon(){
      return window.localStorage.getItem('TOKEN')
    }
  },
  mounted() {
    // 通过全局事件总线将keyword置空
    this.$bus.$on("removeKeyword", () => {
      this.keyword = "";
    });
    // 获取用户登录信息
    this.$store.dispatch("getUserInfo");
  },
  beforeDestroy() {
    // 清除事件总线
    this.$bus.$off("removeKeyword");
  },
  methods: {
    // 退出登录
    async logout() {
      // 发送请求通知服务器
      // 清除项目中的数据
      try {
        await this.$store.dispatch("logout");
        this.$router.push("/login");
      } catch (error) {
        console.log(error.message);
      }
    },
    // 编程式导航，跳转到搜索页面
    goSearch() {
      // 路由传递参数
      // 【1】字符串形式
      // this.$router.push('/search/' + this.keyword)
      //【2】模板字符串形式
      // this.$router.push(`/search/${this.keyword}`)
      //【3】对象形式： params需要给路由取一个名字，query参数可以直接使用路由
      // this.$router.push({
      //     name:'search',
      //     params:{
      //       keyword: this.keyword || undefined
      //     }
      // })

      // 合并query和params参数
      let location = {
        name: "search",
        params: { keyword: this.keyword || undefined },
      };
      if (this.$route.query) {
        location.query = this.$route.query;
      }
      this.$router.push(location);
      // NavigaatationDuplicated错误
      // 1、编程时导航可以给push传递成功和失败的回调函数
      // 2、重写push和replace方法

      // params参数必须使用路由命名，不能使用path
      //   this.$router.push({
      //     name:'SearchName',
      //     params:{
      //       keyword: this.keyword
      //     }
      // })
      // 如果传递的是空串，使用undefined解决
    },
  },
};
</script>

<style scoped lang="less">
.header {
  & > .top {
    background-color: #eaeaea;
    height: 30px;
    line-height: 30px;

    .container {
      width: 1200px;
      margin: 0 auto;
      overflow: hidden;

      .loginList {
        float: left;

        p {
          float: left;
          margin-right: 10px;

          .register {
            border-left: 1px solid #b3aeae;
            padding: 0 5px;
            margin-left: 5px;
          }
        }
      }

      .typeList {
        float: right;

        a {
          padding: 0 10px;

          & + a {
            border-left: 1px solid #b3aeae;
          }
        }
      }
    }
  }

  & > .bottom {
    width: 1200px;
    margin: 0 auto;
    overflow: hidden;

    .logoArea {
      float: left;

      .logo {
        img {
          width: 250px;
          margin: 25px 0px;
        }
      }
    }

    .searchArea {
      float: right;
      margin-top: 35px;

      .searchForm {
        overflow: hidden;

        input {
          box-sizing: border-box;
          width: 490px;
          height: 32px;
          padding: 0px 4px;
          border: 2px solid #ea4a36;
          float: left;

          &:focus {
            outline: none;
          }
        }

        button {
          height: 32px;
          width: 68px;
          background-color: #ea4a36;
          border: none;
          color: #fff;
          float: left;
          cursor: pointer;

          &:focus {
            outline: none;
          }
        }
      }
    }
  }
}
</style>
