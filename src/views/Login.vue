<template>
  <v-card width="400"
          class="mx-auto mt-5">
    <v-card-title class="pb-10">
      <h1 class="display-1 grey--text">Login</h1>
    </v-card-title>
    <v-card-text>
      <v-form>
        <v-text-field v-model="email"
                      type="email"
                      label="Email"
                      prepend-icon="mdi-account-circle"
                      autocomplete="username" />
        <v-text-field v-model="password"
                      :type="showPassword ? 'text' : 'password'"
                      label="Password"
                      prepend-icon="mdi-lock"
                      :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
                      @click:append="showPassword = !showPassword"
                      autocomplete="current-password" />
      </v-form>
      <user-feedback :show="fbShow"
                     :type="fbType"
                     :message="fbMsg">
      </user-feedback>
    </v-card-text>
    <v-card-actions>
      <v-btn @click="login"
             :loading="loading"
             color="success">Login</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
// eslint-disable-next-line
import EventBus from '@/eventBus'
import UserFeedback from '@/components/UserFeedback'

export default {
  name: 'Login',
  data () {
    return {
      loading: false,
      showPassword: false,
      email: null,
      password: null,
      fbShow: false,
      fbType: 'info',
      fbMsg: 'User feedback'
    }
  },
  methods: {
    login () {
      this.fbShow = false
      this.loading = true
      this.$auth.login(this.email, this.password)
      EventBus.$on('ERROR_MESSAGE', payload => {
        this.loading = false
        this.fbMsg = payload
        this.fbType = 'error'
        this.fbShow = true
        if (!this.fbMsg) {
          this.$router.push({ name: 'Home' })
        }
      })
    }
  },
  components: {
    UserFeedback
  }
}
</script>

<style>
</style>
