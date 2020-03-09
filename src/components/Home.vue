<template>
  <el-container class="home-container">
    <el-header class="home-header" style="height: 100px;">
      <div>
        <span>后台管理系统</span>
      </div>
      <el-button type="info" @click="loginOut">退出系统</el-button>
    </el-header>
    <el-container>
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle-btn" @click="toggleCollapse">|||</div>
        <!-- 侧边栏菜单区域 -->
        <el-menu background-color="#001529" text-color="#fff" active-text-color="#409bff" :default-active="activePath" router :index="item.id" :unique-opened="false"  :collapse="isCollapse" :collapse-transition="false"
                 v-for="item in menuList" :key="item.id" >
          <el-submenu :index="item.id + ''">
            <template slot="title">
              <i :class="menuIcons[item.id]" class="menu-icon"></i>
              <span>{{item.authName}}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/' + subItem.path)" >
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{subItem.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('active_path')
  },
  data () {
    return {
      menuList: [],
      menuIcons: {
        125: 'el-icon-s-custom',
        103: 'el-icon-set-up',
        101: 'el-icon-s-shop',
        102: 'el-icon-s-order',
        145: 'el-icon-data-analysis'
      },
      isCollapse: false,
      activePath: ''
    }
  },
  methods: {
    loginOut () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList () {
      const { data: result } = await this.$axios.get('/menu')
      if (result.code === '0') {
        this.menuList = result.data
      } else {
        this.$message.info(result.msg)
      }
    },
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    saveNavState (activePath) {
      window.sessionStorage.setItem('active_path', activePath)
    }
  }
}
</script>

<style lang="less" scoped>
  .el-header {
    background-color: #001529;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: white;
    font-size: 26px;
  }
  .el-aside {
    background-color: #001529;
    .el-menu {
      border-right: none;
    }
  }
  .home-container {
    height: 100%;
  }
  .menu-icon {
    margin-right: 10px;
  }
  .toggle-btn{
    background-color: #4a5064;
    font-size: 10px;
    line-height: 20px;
    color: white;
    text-align: center;
    letter-spacing: 0.3em;
    cursor: pointer;
  }
</style>
