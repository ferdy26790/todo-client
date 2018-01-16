<template>
  <div class="hello">
    <img width="100px" height="100px" src="https://78.media.tumblr.com/95c965a37d17852255d1381f99559dc8/tumblr_nwh316eI561sa67qgo1_500.gif" alt="">
    <div class="container">
      <form v-on:submit.prevent="login" class="login">
      <fieldset>
        <legend>Login</legend>

        <div class="form-group">
          <label for="exampleInputEmail1">Email address</label>
          <input v-model="email" name="username" type="text" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="Enter email">
          <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
        </div>
        <div class="form-group">
          <label for="exampleInputPassword1">Password</label>
          <input v-model="password" name="password" type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
        </div>



        <button v-on:click="login" type="submit" class="btn btn-primary">Submit</button>
      </fieldset>
    </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      url: 'http://localhost:3000/api/'
    }
  },
  methods: {
    login () {
      let self = this
      this.$http.post(this.url + 'login', {
        email: this.email,
        password: this.password
      })
      .then((response) => {
        localStorage.setItem('token', response.data.token)
        self.$router.push('/home')
      }).catch((err) => {
        console.log(err)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
