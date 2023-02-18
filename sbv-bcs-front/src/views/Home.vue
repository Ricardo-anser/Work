<!-- 页面主体框架搭建，使用element-ui的现成框架 -->
<template>
<el-container style="min-height: 100vh;">
  <el-aside :width="sideWidth+'px'" style="background-color: rgb(238, 241, 246);">
    <el-menu :default-openeds="['1', '3']" 
              style="min-height:100%;overflow-x:hidden" 
              background-color="rgb(48,65,86)"
              text-color="#fff"
              active-text-color="#ffd04b"
              :collapse-translation="false"
              :collapse="isCollapse">

       <!-- 最小高度100%，超出部分隐藏，背景颜色为深灰蓝，文字颜色为白色，点击选中文字颜色为黄色 -->
             <!-- collapse数据绑定实现点击图标隐藏侧边栏 -->
             <div style="height:60px;line-height: 60px;text-align: center;">
                  <img src="../assets/logo.png" alt="" style="width: 20px;position: relative;top: 5px;margin-right: 5px;">
                  <b style="color:white" v-show="logoTextShow">后台管理系统</b>
             </div>
      <el-submenu index="1">
        <template slot="title">
            <i class="el-icon-message"></i>
            <span slot="title">导航一</span>
        </template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="1-1">选项1</el-menu-item>
          <el-menu-item index="1-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="1-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="1-4">
          <template slot="title">选项4</template>
          <el-menu-item index="1-4-1">选项4-1</el-menu-item>
        </el-submenu>
      </el-submenu>
      <el-submenu index="2">
        <template slot="title">
            <i class="el-icon-menu"></i>
            <span slot="title">导航二</span>
        </template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="2-1">选项1</el-menu-item>
          <el-menu-item index="2-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="2-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="2-4">
          <template slot="title">选项4</template>
          <el-menu-item index="2-4-1">选项4-1</el-menu-item>
        </el-submenu>
      </el-submenu>
      <el-submenu index="3">
        <template slot="title"><i class="el-icon-setting"></i>
            <span slot="title">导航三</span>
        </template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="3-1">选项1</el-menu-item>
          <el-menu-item index="3-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="3-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="3-4">
          <template slot="title">选项4</template>
          <el-menu-item index="3-4-1">选项4-1</el-menu-item>
        </el-submenu>
      </el-submenu>
    </el-menu>
  </el-aside>
  
  <el-container>
    <el-header style="font-size: 12px; border-bottom: 1px solid #ccc; line-height: 60px; display: flex">
        <div style="flex: 1; font-size: 18px">
          <span :class="collapseBtnClass" style="cursor: pointer;" @click="collapse"></span>
        </div>
        <el-dropdown style="width: 70px; cursor: pointer">
          <span>王小虎</span><span class="el-icon-arrow-down" style="margin-left: 5px"></span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>个人信息</el-dropdown-item>
            <el-dropdown-item>退出</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
    </el-header>
    
    <el-main>
      <!-- 顶部搜索栏实现 -->
      <div style="margin:10px 0;">
        <el-input v-model="username" placeholder="请输入名称" size="mini"  style="width:200px" suffix-icon="el-icon-search"></el-input>
        <el-input  placeholder="请输入邮箱" size="mini" style="width:200px" class="ml-5" suffix-icon="el-icon-message"></el-input>
        <el-input  placeholder="请输入地址" size="mini" style="width:200px" class="ml-5" suffix-icon="el-icon-position"></el-input>
        <el-button type="primary" size="mini" @click="load" class="ml-5">搜索</el-button>
        <el-button type="primary" size="mini" @click="reset" class="ml-5">重置</el-button>
      </div>
      <!-- 顶部操作按钮 -->
      <!-- margin 10px 0上下边距为10px -->
        <!-- 按钮添加 -->
        <div style="margin: 10px 0;">
            <el-button type="primary" @click="handleAdd">新增 <i class="el-icon-circle-plus-outline"></i></el-button>
            <!-- 添加批量删除数据二次弹窗提示  el-popconfirm消息框弹窗组件 -->
            <el-popconfirm
              title="您确定删除这些数据吗?"
              class="ml-5"
              confirmButtonText="确定"
              cancelButtonText="我再想想"
              confirmButtonType="primary"
              icon="el-icon-question"
              iconColor="#f90"
              hideIcon="false"
              @confirm="delBatch">
              <el-button type="danger" slot="reference">批量删除 <i class="el-icon-remove-outline"></i></el-button>
            </el-popconfirm>
          <el-button type="primary" class="ml-5">导入 <i class="el-icon-bottom"></i></el-button>
          <el-button type="primary">导出 <i class="el-icon-top"></i></el-button>
        </div>
      <el-table :data="tableData">
        <el-table-column prop="date" label="日期" width="140">
        </el-table-column>
        <el-table-column prop="name" label="姓名" width="120">
        </el-table-column>
        <el-table-column prop="address" label="地址">
        </el-table-column>
        <el-table-column lable="操作" width="200">
        <template slot-scope="scope">
            <el-button type="success" @click="handleEdit(scope.row)">编辑<i class="el-icon-edit"></i></el-button>
            <el-popconfirm
              title="您确定删除吗?"
              class="ml-5"
              confirmButtonText="确定"
              cancelButtonText="我再想想"
              confirmButtonType="primary"
              cancelButtonType="text"
              icon="el-icon-info"
              iconColor="#f90"
              hideIcon="false"
              @confirm="del(scope.row.id)">
              <el-button type="danger" slot="reference">删除<i class="el-icon-delete"></i></el-button>
            </el-popconfirm>
        </template>
      </el-table-column>
      </el-table>
      <!-- 底边分页实现 -->
      <div style="margin:10px 0;">
        <!-- size-change:改变每次显示条数-->
        <!-- current-change: -->
        <!-- pagesizes:选择显示页数 -->
        <!-- 绑定pagesize -->
        <!-- layout:布局 -->
        <!-- total:全部条数显示 -->
        <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :page-sizes="[2,10,15,20]"
        :page-size="pageSize"
        layout="total,sizes,prev,pager,next,jumper"
        :total="total">
        </el-pagination>  
      </div>

    </el-main>
  </el-container>
</el-container>
</template>

<script>
  export default {
    name:'home',
    data() {
      const item = {
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      };
      return {
        tableData: Array(12).fill(item),
        collapseBtnClass:'el-icon-s-fold',
        isCollapse:false,
        sideWidth:200,
      }
    },
    methods:{
      //侧边栏展开缩放
      collapse(){
        this.isCollapse=!this.isCollapse;
        if(this.isCollapse){
          this.sideWidth=60
          this.collapseBtnClass='el-icon-s-unfold'
          this.logoTextShow=false
        }
        else{
          this.sideWidth=200
          this,this.collapseBtnClass='el-icon-s-fold'
          this.logoTextShow=true
        }
      },
      load(){

      },
      reset(){

      }
    }
  };
</script>
<style>
</style>

