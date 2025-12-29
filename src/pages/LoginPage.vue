<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <q-page class="flex flex-center bg-grey-2">
        <q-card class="q-pa-md shadow-4 login-card" style="width: 360px; max-width: 90vw">
          <q-card-section class="text-center">
            <q-avatar size="80px" class="q-mb-sm shadow-2 bg-primary text-white">
              <q-icon name="admin_panel_settings" />
            </q-avatar>
            <div class="text-h5 text-weight-bold q-mt-sm text-primary">Welcome Back</div>
            <div class="text-caption text-grey">Sign in to Customer Manager</div>
          </q-card-section>

          <q-card-section>
            <q-input
              filled
              v-model="email"
              label="Email Address"
              lazy-rules
              :rules="[(val) => (val && val.length > 0) || 'Please type your email']"
              class="q-mb-md"
            >
              <template v-slot:prepend>
                <q-icon name="email" />
              </template>
            </q-input>

            <q-input
              filled
              type="password"
              v-model="password"
              label="Password"
              lazy-rules
              :rules="[(val) => (val && val.length > 0) || 'Please type your password']"
            >
              <template v-slot:prepend>
                <q-icon name="lock" />
              </template>
            </q-input>

            <div class="row items-center justify-between q-mt-sm">
              <q-checkbox v-model="rememberMe" label="Remember me" color="primary" />
              <a href="#" class="text-primary text-caption" style="text-decoration: none"
                >Forgot Password?</a
              >
            </div>
          </q-card-section>

          <q-card-section>
            <q-btn
              unelevated
              rounded
              color="primary"
              size="lg"
              class="full-width"
              label="Sign In"
              @click="login"
            />
          </q-card-section>
        </q-card>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useQuasar } from 'quasar'

const $q = useQuasar()
const router = useRouter()

const email = ref('')
const password = ref('')
const rememberMe = ref(false)

function login() {
  if (email.value && password.value) {
    $q.loading.show()
    setTimeout(() => {
      $q.loading.hide()
      router.push('/')
      $q.notify({
        type: 'positive',
        message: 'Login Successful',
        position: 'top',
      })
    }, 1000)
  } else {
    $q.notify({
      type: 'negative',
      message: 'Please fill in all fields',
    })
  }
}
</script>

<style scoped>
.login-card {
  border-radius: 16px;
}
</style>
