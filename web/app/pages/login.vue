<script setup lang="ts">
definePageMeta({
  middleware: ['guest'],
  layout: 'blank',
})

useSeoMeta({
  title: 'Log in to DeliSMS Gateway',
  description:
    'Log in to DeliSMS to manage the external Android SMS gateway lab for DeliChow.',
})

const route = useRoute()
const to = computed(() => (route.query.to as string) || '/threads')
</script>

<template>
  <VContainer fluid class="fill-height d-flex align-center justify-center login-page">
    <VRow align="center" justify="center">
      <VCol cols="12" md="5" lg="4" xl="3">
        <div class="text-center mb-6">
          <VImg
            src="/img/delisms-icon-128.png"
            alt="DeliSMS logo"
            width="76"
            height="76"
            class="mx-auto mb-4"
          />
          <h1 class="text-h4 font-weight-bold">DeliSMS Gateway</h1>
          <p class="text-body-1 text-medium-emphasis mt-2 mb-0">
            External Android SMS gateway console for DeliChow lab operations.
          </p>
        </div>

        <VAlert type="warning" variant="tonal" class="mb-4">
          Registration is invite-only. Public social sign-in is disabled for this lab.
        </VAlert>

        <VCard max-width="520" class="mx-auto pa-4">
          <VCardText class="px-4 pt-2 pb-0">
            <ClientOnly>
              <FirebaseAuth :to="to" />
            </ClientOnly>
          </VCardText>
        </VCard>

        <div class="text-center mt-4">
          <BackButton />
        </div>
      </VCol>
    </VRow>
  </VContainer>
</template>

<style scoped>
.login-page {
  background:
    radial-gradient(circle at top left, rgba(255, 122, 26, 0.12), transparent 32rem),
    #101010;
}
</style>
