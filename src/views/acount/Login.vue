<template>
  <div class="account">
    <div class="form-wrap">
        <a-form
            name="custom-validation"
            ref="formRef"
        >
            <a-form-item>
                <label>用户名</label>
                <a-input type="text" autocomplete="off" />
            </a-form-item>
            <a-form-item>
                <label>密码</label>
                <a-input type="password" />
            </a-form-item>
            <a-form-item>
                <div id="captcha-dom" class="nc-container"></div>
            </a-form-item>
            <a-form-item>
                <a-button type="primary" html-type="submit" block>
                    登录
                </a-button>
            </a-form-item>
        </a-form>
        <div class="other text-center">
            <a href="" class="color-white">忘记密码</a> |
            <a href="" class="color-white">注册</a>
        </div>
    </div>
  </div>
</template>
<script>
import { reactive, onMounted } from "vue";
import  "./captcha.js"
export default {
  name: "Login",
  setup(props) {
    const formConfig = reactive({
        layout: {
            labelCol: { span: 4 },
            wrapperCol: { span: 14 },
        }
    })

    onMounted(() => {
        const nc_token = ["FFFF0N00000000009931",(new Date()).getTime(), Math.random].join(':');
        const NC_Opt = {
            renderTo: "captcha-dom",
            appkey: "FFFF0N00000000009931",
            scence: "nc_login",
            token: nc_token,
            customWidth: 300,
            trans: {"key1":"code0"},
            elementID: ["usernameID"],
            is_Opt: 0,
            language: "cn",
            isEnabled: true,
            timeout: 3000,
            times: 5,
            apimap: {

            },
            callback: function (data) {
                window.console && console.log(nc_token)
                window.console && console.log(data.csessionid)
                window.console && console.log(data.sig)
            }
        }

        const nc = new noCaptcha(NC_Opt)
        nc.upLang('cn', {
            _startTEXT: "请按住滑块，拖动到最右边",
            _yesTEXT: "验证通过",
            _error300: "哎呀，出错了，点击<a href=\"javascript:_nc.reset()\">刷新</a>再来一次",
            _errorNetwork: "网络不给力，请<a href=\"javascript:_nc.reset()\">点击刷新</a>"
        })
    })

    return {
        formConfig
    }
  }
};
</script>
<style lang="scss">
    @import "./index.scss";
</style>
