<!-- components/Navbar.vue -->
<template>
  <v-app-bar flat class="main-navbar px-6">
    <v-app-bar-title class="font-weight-bold logo-title">
      <a href="/" class="logo-link">
        <img src="/logo.png" alt="Logo" class="logo-image" />
      </a>
    </v-app-bar-title>

    <v-spacer />

    <v-btn variant="text" href="/" class="home-link">الرئيسية</v-btn>
  </v-app-bar>
</template>

<style scoped>
.main-navbar {
  align-items: center;
}

.logo-title {
  display: flex;
  align-items: center;
}

.logo-link {
  display: inline-flex;
  align-items: center;
}

.logo-image {
  display: block;
  max-height: 40px;
  max-width: 100%;
  width: auto;
  object-fit: contain;
}

.home-link {
  min-width: 0;
}

@media (max-width: 768px) {
  .main-navbar {
    padding: 0 12px;
    min-height: 56px;
  }

  .logo-image {
    max-height: 34px;
  }

  .logo-link {
    padding: 4px 0;
  }

  .home-link {
    font-size: 0.95rem;
  }
}
</style>
