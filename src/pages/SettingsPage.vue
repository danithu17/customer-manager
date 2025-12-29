<template>
  <q-page class="q-pa-md">
    <div class="row q-col-gutter-md">
      <div class="col-12 col-md-4">
        <q-card class="shadow-2">
          <q-card-section class="flex flex-center column">
            <q-avatar size="100px" class="shadow-3 q-mb-md">
              <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
            </q-avatar>
            <div class="text-h6">Admin User</div>
            <div class="text-subtitle2 text-grey">Super Administrator</div>
          </q-card-section>
          <q-card-section>
            <q-list separator>
              <q-item clickable v-ripple>
                <q-item-section avatar><q-icon name="person" color="primary" /></q-item-section>
                <q-item-section>Edit Profile</q-item-section>
                <q-item-section side><q-icon name="chevron_right" /></q-item-section>
              </q-item>
              <q-item clickable v-ripple>
                <q-item-section avatar><q-icon name="lock" color="primary" /></q-item-section>
                <q-item-section>Change Password</q-item-section>
                <q-item-section side><q-icon name="chevron_right" /></q-item-section>
              </q-item>
            </q-list>
          </q-card-section>
        </q-card>
      </div>

      <div class="col-12 col-md-8">
        <q-card class="shadow-2">
          <q-card-section>
            <div class="text-h6">System Settings</div>
          </q-card-section>
          <q-separator />
          <q-card-section>
            <q-list class="q-pa-md">
              <q-item tag="label" v-ripple>
                <q-item-section>
                  <q-item-label>Email Notifications</q-item-label>
                  <q-item-label caption>Receive emails about new customers</q-item-label>
                </q-item-section>
                <q-item-section side>
                  <q-toggle color="primary" v-model="notifications" val="email" />
                </q-item-section>
              </q-item>

              <q-item tag="label" v-ripple>
                <q-item-section>
                  <q-item-label>Dark Mode</q-item-label>
                  <q-item-label caption>Enable system-wide dark theme</q-item-label>
                </q-item-section>
                <q-item-section side>
                  <q-toggle color="black" v-model="darkMode" @update:model-value="toggleDark" />
                </q-item-section>
              </q-item>

              <q-item tag="label" v-ripple>
                <q-item-section>
                  <q-item-label>Maintenance Mode</q-item-label>
                  <q-item-label caption>Prevent users from accessing the system</q-item-label>
                </q-item-section>
                <q-item-section side>
                  <q-toggle color="red" v-model="maintenance" />
                </q-item-section>
              </q-item>
            </q-list>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import { useQuasar } from 'quasar'

const $q = useQuasar()
const notifications = ref(true)
const darkMode = ref(false)
const maintenance = ref(false)

function toggleDark(val) {
  $q.dark.set(val)
}
</script>
