<script setup lang="ts">
import { useDisplay } from 'vuetify'

const { lgAndUp, mdAndUp } = useDisplay()
const authStore = useAuthStore()
const currentYear = new Date().getFullYear()
</script>

<template>
  <v-app>
    <v-app-bar color="#121212" elevation="0">
      <v-container>
        <v-row>
          <v-col class="w-full d-flex align-center">
            <NuxtLink to="/" class="text-decoration-none d-flex align-center">
              <img src="/img/delisms-icon-64.png" alt="DeliSMS logo" class="brand-logo" />
              <div v-if="lgAndUp" class="ml-3">
                <div class="text-h5 font-weight-bold text-white">
                  DeliSMS
                </div>
                <div class="text-caption text-medium-emphasis mt-n1">
                  External SMS Gateway
                </div>
              </div>
            </NuxtLink>

            <v-spacer />

            <v-btn
              v-show="lgAndUp"
              variant="text"
              color="primary"
              class="mr-2"
              href="#how-it-works"
            >
              How it works
            </v-btn>

            <v-btn
              v-show="lgAndUp"
              variant="text"
              color="primary"
              class="mr-2"
              href="#operator-checklist"
            >
              Checklist
            </v-btn>

            <v-btn
              v-if="authStore.authUser === null"
              color="primary"
              variant="flat"
              :class="{ 'mt-1': !mdAndUp }"
              :size="lgAndUp ? 'large' : 'default'"
              to="/login"
            >
              Operator Login
            </v-btn>

            <v-btn
              v-else
              color="primary"
              variant="flat"
              :class="{ 'mt-1': !mdAndUp }"
              :size="lgAndUp ? 'large' : 'default'"
              to="/threads"
            >
              Open Console
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-app-bar>

    <v-main>
      <AppToast />
      <slot />
    </v-main>

    <v-footer color="#121212" class="pt-10 pb-6">
      <v-container>
        <v-row>
          <v-col cols="12" md="5">
            <div class="d-flex align-center mb-4">
              <img src="/img/delisms-icon-64.png" alt="DeliSMS logo" class="brand-logo" />
              <div class="ml-3">
                <div class="text-h5 font-weight-bold text-white">
                  DeliSMS
                </div>
                <div class="text-body-2 text-medium-emphasis">
                  External Android SMS transport gateway for DeliChow.
                </div>
              </div>
            </div>

            <p class="text-body-2 text-medium-emphasis mb-0">
              Lab / foundation service. Delivery depends on the Android device,
              SIM, carrier network, Firebase dispatch, and webhook processing.
            </p>
          </v-col>

          <v-col cols="12" md="3">
            <h2 class="text-subtitle-1 font-weight-bold mb-3 text-white">
              Gateway
            </h2>
            <ul class="footer-list">
              <li>Custom web: sms.delichow.ph</li>
              <li>Custom API: api.sms.delichow.ph</li>
              <li>Android package: com.httpsms</li>
            </ul>
          </v-col>

          <v-col cols="12" md="2">
            <h2 class="text-subtitle-1 font-weight-bold mb-3 text-white">
              Console
            </h2>
            <ul class="footer-list">
              <li>
                <NuxtLink class="footer-link" to="/login">
                  Login
                </NuxtLink>
              </li>
              <li>
                <NuxtLink class="footer-link" to="/threads">
                  Messages
                </NuxtLink>
              </li>
              <li>
                <NuxtLink class="footer-link" to="/phone-api-keys">
                  Phone API Keys
                </NuxtLink>
              </li>
              <li>
                <NuxtLink class="footer-link" to="/settings">
                  Settings
                </NuxtLink>
              </li>
            </ul>
          </v-col>

          <v-col cols="12" md="2">
            <h2 class="text-subtitle-1 font-weight-bold mb-3 text-white">
              Legal
            </h2>
            <ul class="footer-list">
              <li>
                <NuxtLink class="footer-link" to="/terms-and-conditions">
                  Terms
                </NuxtLink>
              </li>
              <li>
                <NuxtLink class="footer-link" to="/privacy-policy">
                  Privacy
                </NuxtLink>
              </li>
            </ul>
          </v-col>
        </v-row>

        <v-divider class="my-6" />

        <div class="text-caption text-medium-emphasis">
          © {{ currentYear }} DeliSMS Gateway Lab. External AGPL gateway service.
          Not yet Paid Client Release ready.
        </div>
      </v-container>
    </v-footer>
  </v-app>
</template>

<style scoped>
.brand-logo {
  height: 42px;
  object-fit: contain;
  width: 42px;
}

.footer-list {
  color: rgba(255, 255, 255, 0.68);
  list-style: none;
  padding: 0;
}

.footer-list li {
  margin-bottom: 8px;
}

.footer-link {
  color: rgba(255, 255, 255, 0.82);
  text-decoration: none;
}

.footer-link:hover {
  color: white;
  text-decoration: underline;
}
</style>
