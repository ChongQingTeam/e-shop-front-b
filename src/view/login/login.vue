<style lang="less">
  @import './login.less';
</style>

<template>
  <div class="login">
    <div class="login-con">
      <Card icon="log-in" title="欢迎登录" :bordered="false">
        <div class="form-con">
          <login-form @on-success-valid="handleSubmit"></login-form>
          <p class="login-tip">如有问题请联系管理人员</p>
        </div>
      </Card>
    </div>
    <Modal
      v-model="showError"
      title="登录报错"
      @on-ok="ok"
      @on-cancel="cancel">
      <p>{{msg}}</p>
    </Modal>
  </div>
</template>

<script>
import LoginForm from '_c/login-form'
import { mapActions } from 'vuex'
import { encrptyAES } from '@/libs/util'

export default {
  data () {
    return {
      showError: false,
      msg: '登录出错，请重试！'
    }
  },
  components: {
    LoginForm
  },
  methods: {
    ...mapActions([
      'handleLogin',
      'getUserInfo'
    ]),
    handleSubmit ({ userName, password }) {
      const that = this
      password = encrptyAES(password)
      this.handleLogin({ userName, password }).then(res => {
        if (res.data.status === '200' && res.data.success === true) {
          this.getUserInfo({ userName }).then(res => {
            that.$router.push({
              name: that.$config.homeName
            }).catch(err => {
              console.info(err)
            })
          })
        } else {
          that.showError = true
          that.msg = res.data.msg
        }
      })
    }
  }
}
</script>

<style>

</style>
