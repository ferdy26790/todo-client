<template>
  <div class="container">

      <fieldset>
        <legend>Register</legend>
        <br>
        <div class="form-group">
          <label>Email address</label>
          <input v-model="email" type="email" class="form-control" aria-describedby="emailHelp" placeholder="Enter email">
          <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
        </div>
        <div class="form-group">
          <label>Name</label>
          <input v-model="name" type="text" class="form-control" placeholder="Enter Name">
        </div>
        <div class="form-group">
          <label>Password</label>
          <input v-model="password" type="password" class="form-control" placeholder="Password">
        </div>
        </fieldset>
        <button v-on:click="register" type="submit" class="btn btn-primary">Register</button>
      </fieldset>

  </div>

</template>

<script>
export default {
  name: 'Register',
  data () {
    return {
      url: 'http://localhost:3000/api/'
    }
  },
  methods: {
    register () {
      let self = this
      this.$http.post(this.url + 'register', {
        name: this.name,
        password: this.password,
        email: this.email
      })
      .then((response) => {
        console.log(response)
        self.$router.push('/')
      }).catch((err) => {
        console.log(err)
      })
    }
  }
}
</script>

<style lang="css">
</style>
