<template>
    <div class="navbar">
        <!-- 汉堡 -->
        <Hamburger class="hamburger-container"></Hamburger>

        <Breadcrumb class="breadcrumb-container"></Breadcrumb>
        <div class="right-menu">
            <HeaderSerch class="right-menu-item hover-effect"></HeaderSerch>
            <screenfull class="right-menu-item hover-effect"></screenfull>
            <ThemeSelect class="right-menu-item hover-effect"></ThemeSelect>
            <lang-select class="right-menu-item hover-effect"></lang-select>

            <!-- 头像 -->
            <el-dropdown class="avatar-container" trigger="click">
                <div class="avatar-wrapper">
                    <el-avatar shape="square" :size="40" :src="$store.getters.userInfo.avatar"></el-avatar>
                    <el-icon>
                        <Setting />
                    </el-icon>
                </div>
                <template #dropdown>
                    <el-dropdown-menu class="user-dropdown">
                        <router-link to="/">
                            <el-dropdown-item>{{ $t('msg.navBar.home') }}</el-dropdown-item>
                        </router-link>
                        <a target="__blank" href="https://coding.imooc.com/class/542.html">
                            <el-dropdown-item>{{ $t('msg.navBar.course') }} </el-dropdown-item>
                        </a>
                        <el-dropdown-item divided @click="logout">{{ $t('msg.navBar.logout') }} </el-dropdown-item>
                    </el-dropdown-menu>
                </template>
            </el-dropdown>
        </div>
    </div>
</template>
<script setup>
import { Setting } from '@element-plus/icons-vue'
import { useStore } from 'vuex'
import Hamburger from '@/components/Hamburger/index.vue'
import Breadcrumb from '@/components/Breadcrumb'
import LangSelect from '@/components/LangSelect'
import ThemeSelect from '@/components/ThemeSelect'
import screenfull from '@/components/Screenfull'
import HeaderSerch from '@/components/HeaderSerch'

const store = useStore()
const logout = () => {
    store.dispatch('user/logout')
}
</script>

<style lang="scss" scoped>
.navbar {
    height: 50px;
    overflow: hidden;
    position: relative;
    background: #fff;
    box-shadow: 0 1px 4px rgba(0, 21, 41, 0.08);

    .hamburger-container {
        line-height: 46px;
        height: 100%;
        float: left;
        cursor: pointer;
        // hover 动画
        transition: background 0.5s;

        &:hover {
            background: rgba(0, 0, 0, 0.1);
        }
    }

    .breadcrumb-container {
        float: left;
    }

    .breadcrumb-container {
        float: left;
    }

    .right-menu {
        display: flex;
        align-items: center;
        float: right;
        padding-right: 16px;

        ::v-deep .right-menu-item {
            display: inline-block;
            padding: 0 18px 0 0;
            font-size: 24px;
            color: #5a5e66;
            vertical-align: text-bottom;

            &.hover-effect {
                cursor: pointer;
                transition: background 0.3s;

                &:hover {
                    background: rgba(0, 0, 0, 0.025);
                }
            }
        }

        ::v-deep .avatar-container {
            cursor: pointer;

            .avatar-wrapper {
                margin-top: 5px;
                position: relative;

                .el-avatar {
                    --el-avatar-background-color: none;
                    margin-right: 12px;
                }
            }
        }
    }
}
</style>