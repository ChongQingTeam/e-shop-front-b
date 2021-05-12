<style lang="less">
  @import './login.less';
</style>

<template>
  <div class="login">
    <div class="login-con">
      <Card icon="log-in" title="欢迎登录" :bordered="false">
        <div class="form-con">
          <login-form @on-success-valid="handleSubmit"></login-form>
          <p class="login-tip">输入任意用户名和密码即可</p>
        </div>
      </Card>
    </div>
  </div>
</template>

<script>
import LoginForm from '_c/login-form'
import { mapActions } from 'vuex'
export default {
  components: {
    LoginForm
  },
  methods: {
    ...mapActions([
      'handleLogin'
      // ,'getUserInfo'
    ]),
    handleSubmit ({ userName, password }) {
      const that = this
      this.handleLogin({ userName, password }).then(res => {
        debugger
        if (res != null && res.data != null) {
          if (res.data.status === '200' && res.data.success === true) {
            that.$router.push({
              name: that.$config.homeName
            }).catch(err => {
              console.info(err)
            })
          }
        }

        // this.getUserInfo().then(res => {

        // })
      })
    }
  }
}
</script>

<style>

</style>
