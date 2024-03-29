<template>
    <div class="login-container">
        <el-form class="login-form" :model="loginForm" :rules="loginRules" ref="loginFromRef">
            <div class="title-container">
                <h3 class="title">{{$t('msg.login.title')}}</h3>
                <lang-select class="lang-select"></lang-select>
            </div>
            <!-- username -->
            <el-form-item prop="username">
                <span class="svg-container">
                    <svg-icon icon="user"></svg-icon>
                </span>
                <el-input placeholder="username" name="username" v-model="loginForm.username"></el-input>
            </el-form-item>
            <!-- password -->
            <el-form-item prop="password">
                <span class="svg-container">
                    <svg-icon icon="password"></svg-icon>
                </span>
                <el-input placeholder="password" name="password" v-model="loginForm.password" :type="passwordType">
                </el-input>
                <span class="show-pwd">
                    <span class="svg-container">
                        <svg-icon :icon="passwordType === 'password' ? 'eye' : 'eye-open'" @click="onChangePwd">
                        </svg-icon>
                    </span>
                </span>
            </el-form-item>
            <!-- 登录按钮 -->
            <el-button type="primary" style="width:100%; margin-bottom: 30px" @click="handlerLogin" :loading="loading">
                {{$t('msg.login.loginBtn')}}</el-button>

            <div class="tips" v-html="$t('msg.login.desc')"></div>
        </el-form>
    </div>
</template>

<script setup>
// 导入的组件可以直接使用
import LangSelect from '@/components/LangSelect'
import { ref } from 'vue'
import { validatePassword } from './rules'
import { useStore } from 'vuex'
import { useI18n } from 'vue-i18n'
// 数据源
const loginForm = ref({
    username: 'super-admin',
    password: '123456'
})
// 验证规则
const i18n = useI18n()
const loginRules = ref({
    username: [
        {
            required: true,
            trigger: 'blur',
            message: i18n.t('msg.login.usernameRule')
        }
    ],
    password: [
        {
            required: true,
            trigger: 'blur',
            validator: validatePassword()
        }
    ]
})
// 处理密码框文本显示
const passwordType = ref('password')
// template 中绑定的方法，直接声明即可
const onChangePwd = () => {
    // 当passwordType 的值为 password 的时候 改为text
    // 使用ref 声明的数据，在script 中使用时，需要加value 来获取具体的值 但是在template中不需要加value
    if (passwordType.value === 'password') {
        passwordType.value = 'text'
    } else {
        passwordType.value = 'password'
    }
}
// 处理登录
const loading = ref(false)
const store = useStore()
const loginFromRef = ref(null)
const handlerLogin = () => {
    // 1.进行表单校验
    console.log(loginFromRef.value)
    loginFromRef.value.validate(valid => {
        if (!valid) return
        // 2.触发登录动作
        loading.value = true
        store.dispatch('user/login', loginForm.value)
            .then(() => {
                loading.value = false
                // 3.进行登录后处理
            })
            .catch(err => {
                console.log(err)
                loading.value = false
            })
    })
}
</script>

<style lang="scss" scoped>
$bg: #2d3a4b;
$dark_gray: #889aa4;
$light_gray: #eee;
$cursor: #fff;

.login-container {
    min-height: 100%;
    width: 100%;
    background-color: $bg;
    overflow: hidden;

    .login-form {
        position: relative;
        width: 520px;
        max-width: 100%;
        padding: 260px 35px 0;
        margin: 0 auto;
        overflow: hidden;

        ::v-deep .el-form-item {
            border: 1px solid rgba(255, 255, 255, 0.1);
            background: rgba(0, 0, 0, 0.1);
            border-radius: 5px;
            color: #454545;
        }

        ::v-deep .el-input {
            display: inline-block;
            height: 47px;
            width: 85%;

            input {
                background: transparent;
                border: 0px;
                border-radius: 0px;
                padding: 12px 5px 12px 15px;
                color: $light_gray;
                height: 47px;
                caret-color: $cursor;
            }
        }

        .tips {
            font-size: 16px;
            color: #fff;
            line-height: 24px;
        }
    }

    .svg-container {
        padding: 6px 5px 6px 15px;
        color: $dark_gray;
        vertical-align: middle;
        display: inline-block;
    }

    .title-container {
        position: relative;

        .title {
            font-size: 26px;
            color: $light_gray;
            margin: 0px auto 40px auto;
            text-align: center;
            font-weight: bold;
        }
    }

    .show-pwd {
        position: absolute;
        right: 10px;
        font-size: 16px;
        color: $dark_gray;
        cursor: pointer;
        user-select: none;
    }

    :deep(.lang-select) {
        position: absolute;
        top: 4px;
        right: 0;
        background-color: #fff;
        font-size: 22px;
        padding: 4px;
        border-radius: 4px;
        cursor: pointer;
    }
}
</style>
