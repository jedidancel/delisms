<script setup lang="ts">
definePageMeta({
  layout: 'website',
})

useSeoMeta({
  title: 'DeliSMS Gateway Lab Notes',
  description:
    'DeliSMS public blog content is temporarily unavailable while the gateway lab is being rebranded and reviewed.',
})
</script>

<template>
  <VContainer class="py-16">
    <VCard class="pa-8">
      <VChip color="primary" variant="tonal" class="mb-4">
        Lab content paused
      </VChip>
      <h1 class="text-h3 font-weight-bold mb-4">DeliSMS Gateway Lab Notes</h1>
      <p class="text-body-1 text-medium-emphasis">
        Public articles are temporarily disabled while this external Android SMS
        gateway is being rebranded, reviewed, and aligned with DeliChow gateway
        lab boundaries.
      </p>
      <VBtn color="primary" class="mt-6" to="/">
        Back to DeliSMS
      </VBtn>
    </VCard>
  </VContainer>
</template>
