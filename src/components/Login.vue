<template>
  <div class="container text-center">
    <form class="form-signin">
      <h1 class="h3 mb-3 font-weight-normal">BKB登录</h1>
      <label for="username" class="sr-only">账号</label>
      <input v-model="username" type="text" id="username" name="username" class="form-control" placeholder="账号" required autofocus>
      <label for="password" class="sr-only">密码</label>
      <input v-model="password" type="password" id="password" name="password" class="form-control" placeholder="密码" required>
      <button class="btn btn-lg btn-primary btn-block" type="button" @click="login()">登录</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      PublicKey: [],
      username: null,
      password: null
    }
  },
  created () {
    this.getPublicKey()
  },
  methods: {
    getPublicKey () {
      this.$http.get('/v2/book/1220562').then(response => {
        this.PublicKey = "MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCyfEuMt351kG8e2ZSN47rp95dY"//response.data
      }).catch(error => {
        console.log(error)
      })
    },
    login () {
      debugger
      this.$JSEncrypt.setPublicKey(this.PublicKey)

      let param = {
        'username': this.username,
        'password': this.$JSEncrypt.encrypt(this.password)
      }

      this.$http.post('/v2/book/1220562', param).then(response => {
        // TODO
      }).catch(error => {
        console.log(error)
      })
    }
  }
}
</script>

<style scoped>
  .form-signin {
    width: 100%;
    max-width: 330px;
    padding: 15px;
    margin: auto;
  }

  .form-signin .form-control {
    position: relative;
    box-sizing: border-box;
    height: auto;
    padding: 10px;
    font-size: 16px;
  }

  .form-signin .form-control:focus {
    z-index: 2;
  }

  .form-signin input[type="email"] {
    margin-bottom: -1px;
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 0;
  }

  .form-signin input[type="password"] {
    margin-bottom: 10px;
    border-top-left-radius: 0;
    border-top-right-radius: 0;
  }
</style>
