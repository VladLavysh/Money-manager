<template>
  <form class="card auth-card" @submit.prevent="submitHandler">
    <div class="card-content">
      <span class="card-title">Home accounting</span>
      <div class="input-field">
        <input id="email" type="text"
              v-model.trim="email"
              :class="{invalid: ($v.email.$dirty && !$v.email.required) || ($v.email.$dirty && !$v.email.email)}"/>
        <label for="email">Email</label>

        <small class="helper-text invalid"
              v-if="$v.email.$dirty && !$v.email.required">
              Email field mustn't be empty
        </small>
        <small class="helper-text invalid"
              v-else-if="$v.email.$dirty && !$v.email.email">
              Enter valid email
        </small>
      </div>
      <div class="input-field">
        <input id="password" type="password"
               v-model.trim="password"
               :class="{invalid: ($v.password.$dirty && !$v.password.required) || ($v.password.$dirty && !$v.password.minLength)}"/>
        <label for="password">Password</label>

        <small class="helper-text invalid"
               v-if="$v.password.$dirty && !$v.password.required">
               Enter password
        </small>
        <small class="helper-text invalid"
               v-if="$v.password.$dirty && !$v.password.minLength">
               The password must contain at least {{$v.password.$params.minLength.min}} characters. Now it contains {{ password.length }} characters
        </small>

      </div>
      <div class="input-field">
        <input id="name" type="text"
               v-model.trim="name"
               :class="{invalid: $v.name.$dirty && !$v.name.required}"/>
        <label for="name">Name</label>
        <small class="helper-text invalid"
               v-if="$v.name.$dirty && !$v.name.required">
               Enter your name
        </small>
      </div>
      <p>
        <label>
          <input type="checkbox" v-model="agreement"/>
          <span>I agree with the rules</span>
        </label>
      </p>
    </div>
    <div class="card-action">
      <div>
        <button class="btn waves-effect waves-light auth-submit" type="submit">
          Sing up
          <i class="material-icons right">send</i>
        </button>
      </div>

      <p class="center">
        Already have an account?
        <router-link to="/login">Enter</router-link>
      </p>
    </div>
  </form>
</template>

<script>
import { email, required, minLength } from 'vuelidate/lib/validators'

export default {
  name: 'register',
  data: () => ({
    email: '',
    password: '',
    name: '',
    agreement: false
  }),

  validations: {
    email: { email, required },
    password: { required, minLength: minLength(6) },
    name: { required },
    agreement: { checked: v => v }
  },

  methods: {
    async submitHandler () {
      if (this.$v.$invalid) {
        this.$v.$touch()
        return
      }

      const formData = {
        email: this.email,
        password: this.password,
        name: this.name
      }

      try {
        await this.$store.dispatch('register', formData)
        this.$router.push('/')
      } catch (e) {}
    }
  }
}
</script>
