<template>
  <div className="page-sign-up">
    <div className="columns">
      <div className="column is-4 is-offset-4">
        <h1 className="title">Sign up</h1>

        <form @submit.prevent="submitForm">
          <div className="field">
            <label>Username</label>
            <div className="control">
              <input type="text" className="input" v-model="username">
            </div>
          </div>

          <div className="field">
            <label>Password</label>
            <div className="control">
              <input type="password" className="input" v-model="password">
            </div>
          </div>

          <div className="field">
            <label>Repeat password</label>
            <div className="control">
              <input type="password" className="input" v-model="password2">
            </div>
          </div>

          <div className="notification is-danger" v-if="errors.length">
            <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
          </div>

          <div className="field">
            <div className="control">
              <button className="button is-dark">Sign up</button>
            </div>
          </div>

          <hr>

          Or
          <router-link to="/log-in">click here</router-link>
          to log in!
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import {toast} from 'bulma-toast'

export default {
  name: 'SignUp',
  data() {
    return {
      username: '',
      password: '',
      password2: '',
      errors: []
    }
  },
  methods: {
    submitForm() {
      this.errors = []

      if (this.username === '') {
        this.errors.push('The username is missing')
      }

      if (this.password === '') {
        this.errors.push('The password is too short')
      }

      if (this.password !== this.password2) {
        this.errors.push('The passwords doesn\'t match')
      }

      if (!this.errors.length) {
        const formData = {
          username: this.username,
          password: this.password
        }

        axios
            .post("/api/v1/users/", formData)
            .then(response => {
              toast({
                message: 'Account created, please log in!',
                type: 'is-success',
                dismissible: true,
                pauseOnHover: true,
                duration: 2000,
                position: 'bottom-right',
              })

              this.$router.push('/log-in')
            })
            .catch(error => {
              if (error.response) {
                for (const property in error.response.data) {
                  this.errors.push(`${property}: ${error.response.data[property]}`)
                }

                console.log(JSON.stringify(error.response.data))
              } else if (error.message) {
                this.errors.push('Something went wrong. Please try again')

                console.log(JSON.stringify(error))
              }
            })
      }
    }
  }
}
</script>