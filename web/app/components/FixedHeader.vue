<script setup lang="ts">
import { useDisplay } from 'vuetify'

const { lgAndUp, mdAndDown } = useDisplay()
</script>

<template>
  <v-app-bar elevation="0" color="#121212" height="70">
    <v-container>
      <v-row>
        <v-col class="w-full d-flex align-center">
          <NuxtLink
            :to="{ name: 'index' }"
            class="text-on-surface text-decoration-none d-flex align-center"
            :class="{ 'mt-2': mdAndDown }"
          >
            <img v-if="lgAndUp" src="/img/delisms-icon-64.png" alt="DeliSMS logo" class="brand-logo mr-3" />
            <div>
              <div class="text-h5 font-weight-bold text-white">DeliSMS</div>
              <div v-if="lgAndUp" class="text-caption text-medium-emphasis mt-n1">
                External SMS Gateway
              </div>
            </div>
          </NuxtLink>

          <v-spacer />

          <v-btn
            color="primary"
            class="mt-3 mb-3"
            :size="lgAndUp ? 'large' : 'default'"
            :to="{ name: 'login' }"
          >
            Operator Login
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-app-bar>
</template>

<style scoped>
.brand-logo {
  height: 36px;
  object-fit: contain;
  width: 36px;
}

</style>
