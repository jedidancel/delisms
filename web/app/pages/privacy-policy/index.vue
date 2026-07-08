<script setup lang="ts">
definePageMeta({
  layout: 'website',
})

useSeoMeta({
  title: 'DeliSMS Privacy Notice',
  description:
    'Privacy notice placeholder for the DeliSMS external Android SMS gateway lab.',
})
</script>

<template>
  <VContainer class="py-16">
    <VCard class="pa-8">
      <VChip color="primary" variant="tonal" class="mb-4">
        Lab privacy notice
      </VChip>

      <h1 class="text-h3 font-weight-bold mb-4">DeliSMS Privacy Notice</h1>

      <p class="text-body-1 text-medium-emphasis">
        DeliSMS is currently an external Android SMS gateway lab for DeliChow.
        This page is a temporary lab notice and must be replaced with reviewed
        legal text before any merchant or paid-client release.
      </p>

      <p class="text-body-1 text-medium-emphasis mt-4">
        The gateway may process account identifiers, phone API keys, Android
        device status, message metadata, SMS content, webhook configuration, and
        delivery/failure events as part of SMS transport validation.
      </p>

      <p class="text-body-1 text-medium-emphasis mt-4">
        DeliChow OS remains responsible for tenant consent rules, customer
        notification policies, billing, quota enforcement, fallback logic, and
        audit ledger ownership.
      </p>

      <VAlert type="warning" variant="tonal" class="mt-6">
        Not yet Paid Client Release ready. Legal, privacy, retention, consent,
        and support terms still require formal review.
      </VAlert>
    </VCard>
  </VContainer>
</template>
