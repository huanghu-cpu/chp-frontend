<template>
  <view class="container">
    <!-- 自定义导航栏容器 - 包含状态栏和导航栏 -->
    <view class="navbar-container" :style="{ height: totalNavBarHeight + 'px' }">
      <!-- 状态栏占位区 -->
      <view class="status-bar" :style="{ height: statusBarHeight + 'px' }"></view>
      
      <!-- 导航栏内容区 -->
      <view class="navbar-content" :style="{ height: navBarHeight + 'px' }">
        <!-- 左侧文字 -->
        <view class="navbar-left">
          <text class="navbar-title">泰格小助手</text>
        </view>
        
        <!-- 中间搜索框 -->
        <view class="navbar-center">
          <view class="search-bar">
            <u-icon name="search" class="search-icon"></u-icon>
            <input class="search-input" placeholder="搜索内容" placeholder-style="color: #a0a0a0;" />
          </view>
        </view>
        
      </view>
    </view>
    
    <!-- 页面内容区域 -->
    <view class="content" :style="{ paddingTop: totalNavBarHeight + 'px' }">
      <text class="welcome-text">欢迎使用泰格小助手</text>
    </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        statusBarHeight: 0, // 状态栏高度
        navBarHeight: 0,    // 导航栏高度
        totalNavBarHeight: 0, // 总导航高度（状态栏+导航栏）
        menuButtonInfo: {}, // 胶囊按钮信息
      }
    },
    onLoad() {
      // 获取系统信息，实现动态适配
      this.getSystemInfo();
    },
    methods: {
      // 获取系统信息并计算导航栏高度
      getSystemInfo() {
        try {
          // 获取系统信息
          const sysInfo = wx.getSystemInfoSync();
          // 获取胶囊按钮信息
          const menuButtonInfo = wx.getMenuButtonBoundingClientRect();
          
          // 设置状态栏高度
          this.statusBarHeight = sysInfo.statusBarHeight;
          // 保存胶囊按钮信息
          this.menuButtonInfo = menuButtonInfo;
          
          // 计算导航栏高度：胶囊按钮高度 + 2*(胶囊按钮上边界 - 状态栏高度)
          // 这个公式可以确保导航栏高度正好包含胶囊按钮，并且上下有相等的边距
          this.navBarHeight = menuButtonInfo.height + 2 * (menuButtonInfo.top - this.statusBarHeight);
          // 计算总导航高度
          this.totalNavBarHeight = this.statusBarHeight + this.navBarHeight;
        } catch (e) {
          console.error('获取系统信息失败:', e);
          // 提供默认值作为降级方案
          this.statusBarHeight = 20;
          this.navBarHeight = 44;
          this.totalNavBarHeight = 64;
        }
      }
    }
  }
</script>

<style scoped>
  .container {
    min-height: 100vh;
    background-color: #f8f9fa;
  }
  
  /* 导航栏容器 - 固定定位 */
  .navbar-container {
    background-color: #2563eb;
    width: 100%;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 1000;
    box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.1);
  }
  
  /* 状态栏占位区 */
  .status-bar {
    background-color: #2563eb;
    width: 100%;
  }
  
  /* 导航栏内容区 - 关键垂直对齐实现 */
  .navbar-content {
    padding: 0 30rpx;
    display: flex;
    align-items: center; /* 核心：垂直居中对齐 */
    justify-content: space-between;
  }
  
  /* 左侧文字容器 */
  .navbar-left {
    flex-shrink: 0;
    display: flex;
    align-items: center;
    height: 100%;
  }
  
  /* 左侧文字样式 */
  .navbar-title {
	font-size: 37rpx;
  	font-weight: 500;
  	font-family: 'Helvetica Neue', 'Arial Rounded MT Bold', '思源柔黑', sans-serif;
  	letter-spacing: 0;
  	line-height: 1.1;
  	/* 圆润友好的渐变配色 */
  	background: linear-gradient(90deg, #bae6fd, #ffffff, #bfdbfe);
  	background-size: 250% auto;
  	-webkit-background-clip: text;
  	-webkit-text-fill-color: transparent;
  	background-clip: text;
  	/* 添加细微的文字发光效果增强可读性 */
  	text-shadow: 0 0 2rpx rgba(255, 255, 255, 0.6);
  	/* 渐变动画效果 */
  	animation: gradientMove 3s ease-in-out infinite;
  }

  /* 渐变动画关键帧 */
@keyframes gradientMove {
  0% {
    background-position: 0% center;
  }
  50% {
    background-position: 100% center;
  }
  100% {
    background-position: 0% center;
  }
}
  
  /* 中间搜索框容器 */
  .navbar-center {
    flex: 1;
    margin: 0 20rpx;
    display: flex;
    align-items: center;
    height: 100%;
  }
  
  /* 搜索框样式 */
  .search-bar {
    width: 55%;
    height: 50rpx;
    background-color: transparent; /* 中间透明 */
    border: 1px solid #000000; /* 细黑边框 */
    border-radius: 36rpx;
    display: flex;
    align-items: center; /* 搜索框内容垂直居中 */
    padding: 0 24rpx; /* 内边距确保内容不紧贴边框 */
    box-shadow: none; /* 移除阴影效果 */
  }
  
  /* 搜索图标 */
  .search-icon {
    font-size: 28rpx;
    color: rgba(128, 128, 128, 0.8);
    margin-right: 12rpx;
  }
  
  /* 搜索输入框 */
  .search-input {
    flex: 1;
    height: 100%;
    background: transparent;
    border: none;
    outline: none;
    color: rgba(128, 128, 128, 0.8);
    font-size: 28rpx;
    line-height: 60rpx; /* 与搜索框高度一致，确保文字垂直居中 */
  }
  
  
  /* 页面内容区域 */
  .content {
    padding: 40rpx 30rpx;
    /* 上边距会通过JS动态设置，确保不被导航栏遮挡 */
  }
  
  .welcome-text {
    font-size: 32rpx;
    color: #666666;
    text-align: center;
    display: block;
    margin-top: 40rpx;
  }
</style>