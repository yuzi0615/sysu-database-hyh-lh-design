<template>
    <div class="app-container">
        <div class="header">
            <div class="header-content">
                <div class="logo">
                    <i class="el-icon-restaurant"></i>
                    <span>中山大学深圳校区东园食堂外卖系统</span>
                </div>
                <div class="user-info">
                    <el-dropdown>
                        <span class="user-dropdown">
                            <i class="el-icon-user"></i>
                            <span class="user-name">{{ username || '用户' }}</span>
                            <i class="el-icon-arrow-down"></i>
                        </span>
                        <el-dropdown-menu slot="dropdown">
                            <el-dropdown-item @click="logout">退出登录</el-dropdown-item>
                        </el-dropdown-menu>
                    </el-dropdown>
                </div>
            </div>
        </div>
        <div class="body">
            <!-- 左侧导航栏 -->
            <div class="sidebar">
                <el-menu 
                    default-active="1" 
                    class="sidebar-menu" 
                    @select="handleselect"
                >
                    <el-menu-item index="1" class="menu-item">
                        <i class="el-icon-menu"></i>
                        <span slot="title">逛店铺</span>
                    </el-menu-item>

                    <el-submenu index="2" class="menu-item">
                        <template slot="title">
                            <i class="el-icon-s-order"></i>
                            <span>个人订单</span>
                        </template>
                        <el-menu-item-group>
                            <el-menu-item index="3">已完成订单</el-menu-item>
                            <el-menu-item index="4">已发货订单</el-menu-item>
                            <el-menu-item index="5">未发货订单</el-menu-item>
                        </el-menu-item-group>
                    </el-submenu>

                    <el-submenu index="8" class="menu-item">
                        <template slot="title">
                            <i class="el-icon-user"></i>
                            <span>个人中心</span>
                        </template>
                        <el-menu-item-group>
                            <el-menu-item index="6">个人信息</el-menu-item>
                            <el-menu-item index="7">修改密码</el-menu-item>
                        </el-menu-item-group>
                    </el-submenu>
                </el-menu>
            </div>
            <div class="main">
                <div id="usershop" v-show="active == 1">
                    <usershop></usershop>
                </div>

                <div id="userfinished" v-show="active == 3">
                    <userfinished></userfinished>
                </div>

                <div id="usersending" v-show="active == 4">
                    <usersending></usersending>
                </div>

                <div id="userunsend" v-show="active == 5">
                    <userunsend></userunsend>
                </div>

                <div id="indimag" v-show="active == 6">
                    <indimsg></indimsg>
                </div>

                <div id="changepwd" v-show="active == 7">
                    <changepwd></changepwd>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
import usershop from '@/components/UserShop.vue'
import userfinished from '@/components/UserOrder/UserFinished.vue'
import usersending from '@/components/UserOrder/UserSending.vue'
import userunsend from '@/components/UserOrder/UserUnsend.vue'
import indimsg from '@/components/UserMsg/IndiMsg.vue'
import changepwd from '@/components/UserMsg/ChPwd.vue'
export default {
    components: {
        usershop: usershop,
        userfinished: userfinished,
        usersending: usersending,
        userunsend: userunsend,
        indimsg: indimsg,
        changepwd: changepwd,
    },
    data() {
        return {
            active: 1,
            username: '',
        };
    },
    mounted() {
        // 从localStorage获取用户名，实际项目中可能需要从后端获取
        this.username = localStorage.getItem('username') || '用户';
    },
    methods: {
        handleselect(index) {
            this.active = index;
        },
        logout() {
            // 清除本地存储的token和用户信息
            localStorage.removeItem('token');
            localStorage.removeItem('username');
            // 跳转到登录页
            this.$router.push('/login');
            // 显示退出登录成功提示
            this.$message({
                message: '退出登录成功',
                type: 'success'
            });
        }
    },
}  
</script>

<style scoped>
.app-container {
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: column;
    background-color: #f5f7fa;
}

/* 头部样式 */
.header {
    width: 100%;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
    z-index: 100;
}

.header-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 70px;
    padding: 0 30px;
}

.logo {
    display: flex;
    align-items: center;
    gap: 12px;
    color: white;
    font-size: 18px;
    font-weight: 600;
    
    i {
        font-size: 24px;
    }
}

.user-info {
    color: white;
    
    .user-dropdown {
        display: flex;
        align-items: center;
        gap: 8px;
        cursor: pointer;
        padding: 8px 12px;
        border-radius: 20px;
        transition: all 0.3s ease;
        
        &:hover {
            background-color: rgba(255, 255, 255, 0.1);
        }
        
        .user-name {
            font-size: 14px;
            font-weight: 500;
        }
    }
}

/* 主体内容 */
.body {
    flex: 1;
    display: flex;
    overflow: hidden;
}

/* 侧边栏 */
.sidebar {
    width: 220px;
    background-color: white;
    box-shadow: 2px 0 8px rgba(0, 0, 0, 0.08);
    transition: all 0.3s ease;
    overflow-y: auto;
}

.sidebar-menu {
    border-right: none;
    height: 100%;
    
    .menu-item {
        margin: 10px 10px 0;
        border-radius: 8px;
        overflow: hidden;
        
        &:first-child {
            margin-top: 20px;
        }
    }
    
    .el-menu-item {
        height: 48px;
        line-height: 48px;
        margin: 0;
        border-radius: 8px;
        transition: all 0.3s ease;
        
        &:hover {
            background-color: rgba(102, 126, 234, 0.1);
        }
        
        &:focus {
            background-color: rgba(102, 126, 234, 0.1);
        }
    }
    
    .el-submenu__title {
        height: 48px;
        line-height: 48px;
        border-radius: 8px;
        transition: all 0.3s ease;
        
        &:hover {
            background-color: rgba(102, 126, 234, 0.1);
        }
    }
    
    .el-submenu .el-menu-item {
        height: 40px;
        line-height: 40px;
        padding-left: 50px !important;
    }
    
    /* 激活状态样式 */
    .el-menu-item.is-active {
        background-color: rgba(102, 126, 234, 0.1) !important;
        color: #667eea !important;
        font-weight: 500;
    }
    
    .el-submenu.is-active > .el-submenu__title {
        background-color: rgba(102, 126, 234, 0.1);
        color: #667eea;
    }
}

/* 主内容区域 */
.main {
    flex: 1;
    padding: 20px;
    overflow-y: auto;
    background-color: #f5f7fa;
}

/* 响应式设计 */
@media (max-width: 1024px) {
    .sidebar {
        width: 180px;
    }
    
    .header-content {
        padding: 0 20px;
    }
    
    .logo {
        font-size: 16px;
        
        i {
            font-size: 20px;
        }
    }
}

@media (max-width: 768px) {
    .sidebar {
        width: 100px;
        
        span {
            display: none;
        }
        
        .el-submenu__title {
            text-align: center;
        }
        
        .el-menu-item {
            text-align: center;
            padding: 0 !important;
        }
        
        .el-submenu .el-menu-item {
            padding: 0 !important;
        }
    }
    
    .logo span {
        display: none;
    }
    
    .main {
        padding: 15px;
    }
}
</style>