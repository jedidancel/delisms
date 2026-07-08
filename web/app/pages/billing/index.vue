<script setup lang="ts">
definePageMeta({
  layout: 'website',
})

useSeoMeta({
  title: 'DeliSMS Billing - Not Enabled',
  description:
    'Billing is not enabled for the DeliSMS gateway lab. Tenant entitlement and billing remain owned by DeliChow OS.',
})
</script>

<template>
  <VContainer class="py-16">
    <VCard class="pa-8">
      <VChip color="warning" variant="tonal" class="mb-4">
        Billing disabled
      </VChip>
      <h1 class="text-h3 font-weight-bold mb-4">Billing is not enabled here</h1>
      <p class="text-body-1 text-medium-emphasis">
        DeliSMS is currently a lab/foundation gateway. Tenant entitlement,
        quotas, pricing, and billing rules remain owned by DeliChow OS, not this
        external gateway console.
      </p>
      <VBtn color="primary" class="mt-6" to="/">
        Back to DeliSMS
      </VBtn>
    </VCard>
  </VContainer>
</template>
