<template>
    <el-container>
        <!-- 头部 -->
        <el-header>
            <div class="words">
                <span @click="menuClick('adminLayout')">
                    <i class="iconfont icon-r-love" style="font-size: 26px">
                        医院管理系统</i
                    >
                </span>
            </div>
            <div class="words">
                <span
                    >欢迎您，<b>{{ userName }}</b
                    >&nbsp;管理员&nbsp;</span
                >
                <span
                    ><el-button type="danger" size="mini" @click="logout">
                        退出</el-button
                    ></span
                >
            </div>
        </el-header>
        <el-container>
            <!-- 侧边栏 -->
            <el-aside width="200px">
                <!-- 导航菜单 -->
                <el-menu
                    background-color="white"
                    text-color="grey"
                    active-text-color="black"
                    :default-active="activePath"
                >
                    <el-menu-item
                        index="adminLayout"
                        @click="menuClick('adminLayout')"
                        style="font-size: 20px"
                    >
                        <i class="iconfont icon-r-home" style="font-size: 26px">
                        </i>
                        首页
                    </el-menu-item>
                    <el-menu-item
                        index="doctorList"
                        @click="menuClick('doctorList')"
                        style="font-size: 20px"
                    >
                        <i
                            class="iconfont icon-r-user1"
                            style="font-size: 26px"
                        >
                        </i>
                        医生信息管理
                    </el-menu-item>
                    <el-menu-item
                        index="patientList"
                        @click="menuClick('patientList')"
                        style="font-size: 20px"
                    >
                        <i
                            class="iconfont icon-r-user2"
                            style="font-size: 26px"
                        >
                        </i>
                        患者信息管理
                    </el-menu-item>
                    <el-menu-item
                        index="orderList"
                        @click="menuClick('orderList')"
                        style="font-size: 20px"
                    >
                        <i
                            class="iconfont icon-r-paper"
                            style="font-size: 26px"
                        >
                        </i>
                        挂号信息管理
                    </el-menu-item>
                    <el-menu-item
                        index="drugList"
                        @click="menuClick('drugList')"
                        style="font-size: 20px"
                    >
                        <i class="iconfont icon-r-love" style="font-size: 26px">
                        </i>
                        药物信息管理
                    </el-menu-item>
                    <el-menu-item
                        index="checkList"
                        @click="menuClick('checkList')"
                        style="font-size: 20px"
                    >
                        <i class="iconfont icon-r-edit" style="font-size: 26px">
                        </i>
                        检查项目管理
                    </el-menu-item>
                    <el-menu-item
                        index="bedList"
                        @click="menuClick('bedList')"
                        style="font-size: 20px"
                    >
                        <i class="iconfont icon-r-list" style="font-size: 26px">
                        </i>
                        病床信息管理
                    </el-menu-item>
                    <el-menu-item
                        index="arrangeIndex"
                        @click="menuClick('arrangeIndex')"
                        style="font-size: 20px"
                    >
                        <i
                            class="iconfont icon-r-shield"
                            style="font-size: 26px"
                        >
                        </i>
                        排班信息管理
                    </el-menu-item>
<!--                    <el-menu-item-->
<!--                        index="dataExpore"-->
<!--                        @click="menuClick('dataExpore')"-->
<!--                        style="font-size: 20px"-->
<!--                    >-->
<!--                        <i-->
<!--                            class="iconfont icon-r-mark1"-->
<!--                            style="font-size: 26px"-->
<!--                        >-->
<!--                        </i>-->
<!--                        数据统计分析-->
<!--                    </el-menu-item>-->
                </el-menu>
            </el-aside>
            <el-main>
                <!-- 子路由入口 -->
                <router-view>
                    <div>你好吗</div>
                </router-view>
            </el-main>
        </el-container>
    </el-container>
</template>
<script>
import jwtDecode from "jwt-decode";
import { toLoad } from "@/utils/initialize.js";
import {
    getToken,
    clearToken,
    getActivePath,
    setActivePath,
} from "@/utils/storage.js";
export default {
    name: "Admin",
    data() {
        return {
            userName: "",
            activePath: "",
        };
    },
    methods: {
        //token解码
        tokenDecode(token) {
            if (token !== null) return jwtDecode(token);
        },
        //导航栏点击事件
        menuClick(path) {
            this.activePath = path;
            setActivePath(path);
            if (this.$route.path !== "/" + path) this.$router.push(path);
            console.log(path);
        },
        //退出登录
        logout() {
            this.$confirm("此操作将退出登录, 是否继续?", "提示", {
                confirmButtonText: "确定",
                cancelButtonText: "取消",
                type: "warning",
            })
                .then(() => {
                    clearToken();
                    this.$message({
                        type: "success",
                        message: "退出登录成功!",
                    });
                    this.$router.push("login");
                })
                .catch(() => {
                    this.$message({
                        type: "info",
                        message: "已取消",
                    });
                });
        },
    },
    mounted() {
        toLoad();
    },
    created() {
        //  获取激活路径
        this.activePath = getActivePath();
        // 解码token
        this.userName = this.tokenDecode(getToken()).aName;
    },
};
</script>
<style scoped lang="scss">
.title {
    cursor: pointer;
}
.el-header {
    background-color: white;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .words {
        text-align: center;
        span {
            color: black;
        }
    }
    border-bottom: 1px solid lightgrey;
}
.el-container {
    height: 100%;
}
.el-aside {
    background-color: white;
    border-right: 1px solid lightgrey;
}
.el-menu {
    border: 0;
}
</style>
