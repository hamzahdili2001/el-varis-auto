<template>
  <v-app>
    <Navbar />

    <v-main>
      <HeroSection />
      <FeaturedCars />
    </v-main>

    <v-footer class="app-footer mt-15">
      <v-container fluid class="py-8">
        <v-row justify="center" align="center" class="footer-row">

          <v-col cols="12" md="4" class="text-center social-col">
            <v-btn icon class="social-btn" href="https://wa.me/966501234567" target="_blank" rel="noopener">
              <v-icon>mdi-whatsapp</v-icon>
            </v-btn>
            <v-btn icon class="social-btn" href="#" target="_blank" rel="noopener">
              <v-icon>mdi-facebook</v-icon>
            </v-btn>
            <v-btn icon class="social-btn" href="#" target="_blank" rel="noopener">
              <v-icon>mdi-instagram</v-icon>
            </v-btn>
          </v-col>
        </v-row>

        <v-row justify="center" class="mt-6">
          <v-col cols="12" class="text-center footer-note">
            © 2026 تأجير السيارات. جميع الحقوق محفوظة.
          </v-col>
        </v-row>
      </v-container>
    </v-footer>
  </v-app>
</template>

<script setup>
import Navbar from './components/Navbar.vue'
import HeroSection from './components/HeroSection.vue'
import FeaturedCars from './components/FeaturedCars.vue'
</script>

<style scoped>
.app-footer {
  background: linear-gradient(180deg, #071028, #031022);
  color: #fff;
  border-top: 1px solid rgba(255, 255, 255, 0.03);
}

.footer-row {
  align-items: center;
}

.footer-links .v-btn {
  color: rgba(255, 255, 255, 0.92) !important;
  margin-inline: 8px;
}

.social-col {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-top: 8px;
}

.social-btn {
  background: rgba(255, 255, 255, 0.06);
  color: #fff !important;
  border-radius: 8px;
  width: 44px;
  height: 44px;
}

.social-btn:hover {
  background: rgba(255, 255, 255, 0.12);
}

.footer-note {
  color: rgba(255, 255, 255, 0.6);
  font-size: 0.95rem;
}

@media (max-width: 768px) {
  .footer-links {
    margin-bottom: 6px;
  }

  .social-col {
    margin-top: 0;
  }
}
</style>
