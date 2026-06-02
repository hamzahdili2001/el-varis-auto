<!-- components/Navbar.vue -->
<script setup>
import { ref } from 'vue'

const drawer = ref(false)
</script>

<template>
  <v-app-bar flat class="main-navbar px-6">
    <v-app-bar-title class="font-weight-bold logo-title">
      <img src="/public/logo.png" alt="Logo" height="32" class="mr-2" />
    </v-app-bar-title>

    <v-spacer />

    <div class="nav-links">
      <v-btn variant="text" to="/">الرئيسية</v-btn>
      <v-btn variant="text" to="/cars">السيارات</v-btn>
      <v-btn color="primary">
        تسجيل الدخول
      </v-btn>
    </div>

    <v-btn icon class="nav-toggle" @click="drawer = !drawer">
      <v-icon>mdi-menu</v-icon>
    </v-btn>
  </v-app-bar>

  <v-navigation-drawer v-model="drawer" temporary right class="mobile-drawer">
    <v-list>
      <v-list-item link to="/" @click="drawer = false">
        <v-list-item-title>الرئيسية</v-list-item-title>
      </v-list-item>
      <v-list-item link to="/cars" @click="drawer = false">
        <v-list-item-title>السيارات</v-list-item-title>
      </v-list-item>
      <v-list-item @click="drawer = false">
        <v-list-item-title>تسجيل الدخول</v-list-item-title>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<style scoped>
.main-navbar {
  align-items: center;
}

.logo-title {
  display: flex;
  align-items: center;
}

.nav-links {
  display: flex;
  align-items: center;
  gap: 10px;
}

.nav-toggle {
  display: none;
}

.mobile-drawer {
  width: 250px;
}

@media (max-width: 768px) {
  .nav-links {
    display: none;
  }

  .nav-toggle {
    display: inline-flex;
  }

  .main-navbar {
    padding: 0 12px;
  }
}

.main-navbar .v-btn {
  min-width: 100px;
}

@media (max-width: 768px) {
  .main-navbar .v-btn {
    width: auto;
    min-width: auto;
  }
}
</style>
