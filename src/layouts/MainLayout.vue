<template>
  <q-layout view="lHh Lpr lFf" class="bg-grey-1">
    <!-- Custom Modern Header -->
    <q-header
      class="bg-white text-grey-9 q-py-xs shadow-1"
      style="border-bottom: 1px solid #f0f0f0"
    >
      <q-toolbar>
        <q-btn flat dense round icon="menu_open" color="primary" @click="toggleLeftDrawer" />

        <q-toolbar-title class="text-weight-bold text-primary flex items-center">
          <q-icon name="dataset" size="28px" class="q-mr-sm" />
          <span style="letter-spacing: -0.5px"
            >NEXUS<span class="text-weight-light">CRM</span></span
          >
        </q-toolbar-title>

        <q-space />

        <div class="q-gutter-sm row items-center no-wrap">
          <q-btn round flat color="grey-7" icon="search">
            <q-tooltip>Search</q-tooltip>
          </q-btn>
          <q-btn round flat color="grey-7" icon="notifications_none">
            <q-badge color="red" floating rounded mini />
            <q-tooltip>Notifications</q-tooltip>
          </q-btn>
          <q-separator vertical inset class="q-mx-sm" />
          <q-avatar size="36px" class="cursor-pointer q-ml-sm">
            <img src="https://cdn.quasar.dev/img/avatar4.jpg" />
            <q-menu>
              <q-list style="min-width: 150px">
                <q-item clickable v-close-popup>
                  <q-item-section>Profile</q-item-section>
                </q-item>
                <q-item clickable v-close-popup to="/login">
                  <q-item-section class="text-red">Logout</q-item-section>
                </q-item>
              </q-list>
            </q-menu>
          </q-avatar>
        </div>
      </q-toolbar>
    </q-header>

    <!-- Custom Dark Sidebar -->
    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="260"
      class="bg-dark-sidebar text-white"
    >
      <div class="column full-height">
        <div class="q-pa-md q-mb-md">
          <div class="text-overline text-grey-5 q-mb-xs">MAIN MENU</div>

          <q-list padding class="menu-list">
            <q-item clickable v-ripple to="/" exact active-class="active-menu-item">
              <q-item-section avatar>
                <q-icon name="dashboard" />
              </q-item-section>
              <q-item-section>
                <q-item-label>Overview</q-item-label>
              </q-item-section>
            </q-item>

            <q-item clickable v-ripple to="/customers" active-class="active-menu-item">
              <q-item-section avatar>
                <q-icon name="people_alt" />
              </q-item-section>
              <q-item-section>
                <q-item-label>Customers</q-item-label>
              </q-item-section>
            </q-item>

            <q-item clickable v-ripple to="/invoices" active-class="active-menu-item">
              <q-item-section avatar>
                <q-icon name="receipt_long" />
              </q-item-section>
              <q-item-section>
                <q-item-label>Invoices</q-item-label>
              </q-item-section>
              <q-item-section side>
                <q-badge color="orange" label="New" />
              </q-item-section>
            </q-item>
          </q-list>
        </div>

        <q-space />

        <div class="q-pa-md">
          <div class="text-overline text-grey-5 q-mb-xs">SYSTEM</div>
          <q-list padding class="menu-list">
            <q-item clickable v-ripple to="/settings" active-class="active-menu-item">
              <q-item-section avatar>
                <q-icon name="tune" />
              </q-item-section>
              <q-item-section>Settings</q-item-section>
            </q-item>
          </q-list>

          <div class="q-mt-md rounded-borders bg-white-translucent q-pa-md text-center">
            <div class="text-caption text-grey-4">Need Help?</div>
            <q-btn flat dense no-caps label="Contact Support" color="white" size="sm" />
          </div>
        </div>
      </div>
    </q-drawer>

    <q-page-container>
      <router-view v-slot="{ Component }">
        <transition name="fade-slide" mode="out-in">
          <component :is="Component" />
        </transition>
      </router-view>
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref } from 'vue'

const leftDrawerOpen = ref(false)

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value
}
</script>

<style scoped>
.bg-dark-sidebar {
  background-color: #1e1e2d;
}

.menu-list .q-item {
  border-radius: 8px;
  margin-bottom: 4px;
  color: #a2a3b7;
  transition: all 0.3s ease;
}

.menu-list .q-item:hover {
  color: white;
  background: rgba(255, 255, 255, 0.05);
}

.active-menu-item {
  background: #3699ff !important;
  color: white !important;
  box-shadow: 0 4px 15px rgba(54, 153, 255, 0.3);
}

.bg-white-translucent {
  background: rgba(255, 255, 255, 0.1);
}

/* Page Transition */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.3s ease;
}

.fade-slide-enter-from,
.fade-slide-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>
