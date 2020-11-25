<template>
<el-container class="home-container">
  <el-header>
  <div>
  <img src="../assets/1.png" alt="">
  <span>电商后台管理系统</span>
  </div>
  <el-button type="info" @click="logout">退出</el-button>
  </el-header>
  <el-container>
    <el-aside :width="isCollapse ? '64px' : '200px'">
    <div class="toggle-button" @click="toggleCollapse">|||</div>
     <el-menu
      background-color="#333744"
      text-color="#fff"
      active-text-color="#409EFF" :unique-opened="true" :collapse="isCollapse" :collapse-transition="false" router :default-active="activePath">
      <el-submenu :index="item.id+''" v-for="item in menulist" :key="item.id">
        <template slot="title">
          <i :class="iconsObj[item.id]"></i>
          <span>{{item.authName}}</span>
        </template>

          <el-menu-item :index="'/'+ subItem.path+''" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/'+subItem.path)">
        <template slot="title">
          <i class="el-icon-menu"></i>
          <span>{{subItem.authName}}</span>
        </template>
          </el-menu-item>
      </el-submenu>
    </el-menu>
    </el-aside>
    <el-main><router-view></router-view></el-main>
  </el-container>
</el-container>
</template>

<script>
  export default {
    data(){
      return{
        menulist:[],
        iconsObj:{
          '125':'iconfont icon-user',
          '103':'iconfont icon-cube',
          '101':'iconfont icon-goods',
          '102':'iconfont icon-doc',
          '145':'iconfont icon-forms'
        },
        isCollapse: false,
        activePath:''
      }
    },
    created(){
       this.getMenuList()
       this.activePath = window.sessionStorage.getItem('activePath')
    },
    methods:{
      logout(){
      window.sessionStorage.clear();
      this.$router.push('/login')
      }, // 获取所有的菜单
     async getMenuList(){
       const {data: res} = await this.$http.get('menus')
       if(res.meta.status !==200) return this.$message.error(res.meta.msg)
       this.menulist = res.data
       console.log(res)
      },
      // 点击按钮实现菜单的折叠与展开
      toggleCollapse(){
        this.isCollapse = !this.isCollapse
      },
      // 保存链接的激活状态
      saveNavState(activePath){
        window.sessionStorage.setItem('activePath',activePath)
        this.activePath = activePath
      }
    }
  }
</script>

<style lang="less" scoped>
.home-container{
  height: 100%;
}
.el-header{
  background: #373d41;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color:#fff;
  font-size: 20px;
  > div{
    display: flex;
    align-items: center;
    > span{
      margin-left: 15px;
    }
  }
}
.el-aside{
  background: #333744;
  > .el-menu{
  border-right: none;
}
}
.el-main{
  background: #eaedf1;
}
.iconfont{
  margin:8px;
}
.toggle-button{
  background: #4A5064;
  color:#fff;
  line-height: 24px;
  font-size:10px;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;

}
</style>
