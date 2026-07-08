<script setup lang="ts">
definePageMeta({
  layout: 'website',
})

useSeoMeta({
  title: 'DeliSMS Terms Notice',
  description:
    'Terms placeholder for the DeliSMS external Android SMS gateway lab.',
})
</script>

<template>
  <VContainer class="py-16">
    <VCard class="pa-8">
      <VChip color="warning" variant="tonal" class="mb-4">
        Lab terms placeholder
      </VChip>

      <h1 class="text-h3 font-weight-bold mb-4">DeliSMS Terms Notice</h1>

      <p class="text-body-1 text-medium-emphasis">
        DeliSMS is currently a lab/foundation gateway and is not yet approved
        for merchant rollout or paid-client release.
      </p>

      <p class="text-body-1 text-medium-emphasis mt-4">
        The Android phone, SIM, carrier plan, device power state, network
        connection, Android background restrictions, Firebase dispatch, and
        webhook receiver can all affect SMS delivery behavior.
      </p>

      <p class="text-body-1 text-medium-emphasis mt-4">
        This gateway remains an external service. It must be integrated with
        DeliChow OS only through HTTP APIs, webhooks, and a DeliChow-owned SMS
        Gateway Adapter.
      </p>

      <VAlert type="warning" variant="tonal" class="mt-6">
        Do not treat this page as final legal terms. Formal terms, privacy,
        merchant responsibility notices, support procedures, and data retention
        rules are still required.
      </VAlert>
    </VCard>
  </VContainer>
</template>
