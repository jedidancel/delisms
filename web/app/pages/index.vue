<script setup lang="ts">
definePageMeta({
  layout: 'website',
})

useSeoMeta({
  title: 'DeliSMS Gateway Lab - External Android SMS Gateway for DeliChow',
  description:
    'DeliSMS is an external Android-based SMS transport gateway for DeliChow lab validation, device pairing, SMS dispatch, and webhook proof.',
  ogTitle: 'DeliSMS Gateway Lab',
  ogDescription:
    'External Android SMS gateway foundation for DeliChow. Lab status, proof required, not yet Paid Client Release ready.',
  ogImage: '/img/delisms-icon-256.png',
  twitterCard: 'summary_large_image',
})
</script>

<template>
  <div class="landing-page">
    <section class="hero-section">
      <VContainer>
        <VRow align="center" class="py-16">
          <VCol cols="12" md="7">
            <VChip color="primary" variant="tonal" class="mb-6">
              Lab / Foundation Gateway
            </VChip>

            <h1 class="hero-title">
              DeliSMS external Android SMS gateway for DeliChow.
            </h1>

            <p class="hero-copy">
              DeliSMS is a self-hosted gateway lab that connects DeliChow SMS
              orchestration to an Android phone, carrier SMS, and webhook events.
              It is designed as a separate external service, not as code inside
              the closed-source DeliChow SaaS repo.
            </p>

            <div class="d-flex flex-wrap ga-3 mt-8">
              <VBtn color="primary" size="large" to="/login">
                Operator Login
              </VBtn>
              <VBtn variant="tonal" size="large" href="#how-it-works">
                View Gateway Flow
              </VBtn>
            </div>

            <VAlert
              type="warning"
              variant="tonal"
              class="mt-8"
              title="Not yet Paid Client Release ready"
            >
              This gateway is currently a lab/foundation service. Merchant
              rollout requires stronger onboarding, monitoring, quota controls,
              failure alerts, support playbooks, and DeliChow OS integration.
            </VAlert>
          </VCol>

          <VCol cols="12" md="5">
            <VCard class="status-card pa-6" elevation="10">
              <div class="text-overline text-primary mb-2">
                Current lab proof
              </div>
              <h2 class="text-h4 font-weight-bold mb-6">
                Custom-domain smoke passed
              </h2>

              <div class="proof-row">
                <span>Web console</span>
                <strong>sms.delichow.ph</strong>
              </div>
              <div class="proof-row">
                <span>Gateway API</span>
                <strong>api.sms.delichow.ph</strong>
              </div>
              <div class="proof-row">
                <span>Android app</span>
                <strong>DeliSMS</strong>
              </div>
              <div class="proof-row">
                <span>SMS dispatch</span>
                <strong>Passed</strong>
              </div>
              <div class="proof-row">
                <span>Webhook event</span>
                <strong>message.phone.delivered</strong>
              </div>
            </VCard>
          </VCol>
        </VRow>
      </VContainer>
    </section>

    <section id="how-it-works" class="py-16">
      <VContainer>
        <div class="section-heading">
          <VChip color="primary" variant="tonal" class="mb-4">
            Gateway flow
          </VChip>
          <h2>How DeliSMS fits the DeliChow SMS path</h2>
          <p>
            The gateway owns SMS transport and Android device communication.
            DeliChow OS remains responsible for tenant rules, notification
            orchestration, billing logic, consent, quotas, fallback decisions,
            and audit records.
          </p>
        </div>

        <VRow class="mt-8">
          <VCol cols="12" md="4">
            <VCard class="info-card pa-6" height="100%">
              <div class="step-number">01</div>
              <h3>DeliChow sends through an adapter</h3>
              <p>
                DeliChow OS should call DeliSMS through HTTP APIs only. Gateway
                source must remain separate from the closed-source SaaS repo.
              </p>
            </VCard>
          </VCol>

          <VCol cols="12" md="4">
            <VCard class="info-card pa-6" height="100%">
              <div class="step-number">02</div>
              <h3>DeliSMS dispatches to Android</h3>
              <p>
                The gateway sends through Firebase Cloud Messaging to the paired
                Android phone, which then sends the carrier SMS using its SIM.
              </p>
            </VCard>
          </VCol>

          <VCol cols="12" md="4">
            <VCard class="info-card pa-6" height="100%">
              <div class="step-number">03</div>
              <h3>Events return by webhook</h3>
              <p>
                Delivery, failure, expiration, and device events must return to
                DeliChow through signed webhook callbacks for audit and support.
              </p>
            </VCard>
          </VCol>
        </VRow>

        <VCard class="flow-card pa-6 mt-8">
          <div class="flow-text">
            DeliChow OS → Notification Orchestrator → SMS Gateway Adapter →
            DeliSMS Gateway → FCM → Tenant Android Phone → Carrier SMS →
            Webhook back to DeliChow
          </div>
        </VCard>
      </VContainer>
    </section>

    <section class="py-16 surface-section">
      <VContainer>
        <div class="section-heading">
          <VChip color="primary" variant="tonal" class="mb-4">
            Console purpose
          </VChip>
          <h2>Built for operational visibility</h2>
          <p>
            The console should help operators see device state, send test SMS,
            inspect messages, verify webhooks, and diagnose failures without
            guessing.
          </p>
        </div>

        <VRow class="mt-8">
          <VCol cols="12" md="3">
            <VCard class="metric-card pa-5" height="100%">
              <h3>Phones</h3>
              <p>Connection status, heartbeat, pairing state, and last activity.</p>
            </VCard>
          </VCol>

          <VCol cols="12" md="3">
            <VCard class="metric-card pa-5" height="100%">
              <h3>Messages</h3>
              <p>Recipient, sender phone, status, timestamps, and failure reason.</p>
            </VCard>
          </VCol>

          <VCol cols="12" md="3">
            <VCard class="metric-card pa-5" height="100%">
              <h3>Webhooks</h3>
              <p>Callback URL, signed event status, issuer, and delivery result.</p>
            </VCard>
          </VCol>

          <VCol cols="12" md="3">
            <VCard class="metric-card pa-5" height="100%">
              <h3>API Keys</h3>
              <p>Phone API keys, linked phones, rotation, and revoke workflow.</p>
            </VCard>
          </VCol>
        </VRow>
      </VContainer>
    </section>

    <section id="operator-checklist" class="py-16">
      <VContainer>
        <VRow>
          <VCol cols="12" md="6">
            <div class="section-heading text-left">
              <VChip color="primary" variant="tonal" class="mb-4">
                Setup checklist
              </VChip>
              <h2>Required proof before any DeliChow OS integration</h2>
              <p>
                A polished screen is not enough. Gateway integration should stay
                blocked until runtime proof is captured and repeatable.
              </p>
            </div>
          </VCol>

          <VCol cols="12" md="6">
            <VList class="checklist-card" lines="two">
              <VListItem title="Deploy API and web services" subtitle="Cloud Run services reachable on custom domains." />
              <VListItem title="Pair Android phone" subtitle="Phone API Key login, heartbeat, and FCM token confirmed." />
              <VListItem title="Send test SMS" subtitle="Carrier SMS received by the target number." />
              <VListItem title="Verify webhook" subtitle="Signed callback received with message.phone.delivered." />
              <VListItem title="Record failure behavior" subtitle="Offline, expired, failed, and webhook error paths documented." />
            </VList>
          </VCol>
        </VRow>
      </VContainer>
    </section>

    <section class="py-16 surface-section">
      <VContainer>
        <VRow>
          <VCol cols="12" md="6">
            <VCard class="notice-card pa-6" height="100%">
              <h2>Merchant responsibility notice</h2>
              <p>
                The Android phone must stay powered, connected, logged in, and
                allowed to run in the background. The SIM must have sufficient
                carrier plan/load. Carrier network issues and Android battery
                restrictions can affect delivery.
              </p>
            </VCard>
          </VCol>

          <VCol cols="12" md="6">
            <VCard class="notice-card pa-6" height="100%">
              <h2>Licensing and repo boundary</h2>
              <p>
                DeliSMS remains an external gateway service. Do not copy AGPL
                gateway source into the closed-source DeliChow SaaS repo. Use
                HTTP APIs, webhooks, and a DeliChow-owned adapter layer.
              </p>
            </VCard>
          </VCol>
        </VRow>
      </VContainer>
    </section>
  </div>
</template>

<style scoped>
.landing-page {
  background:
    radial-gradient(circle at top left, rgba(255, 122, 26, 0.16), transparent 34rem),
    #0f0f0f;
  color: white;
}

.hero-section {
  background: linear-gradient(180deg, #121212 0%, #161616 100%);
}

.hero-title {
  font-size: clamp(2.5rem, 6vw, 5.4rem);
  font-weight: 900;
  letter-spacing: -0.07em;
  line-height: 0.95;
  max-width: 880px;
}

.hero-copy {
  color: rgba(255, 255, 255, 0.72);
  font-size: 1.22rem;
  line-height: 1.8;
  margin-top: 28px;
  max-width: 780px;
}

.status-card,
.info-card,
.metric-card,
.notice-card,
.flow-card,
.checklist-card {
  background: rgba(255, 255, 255, 0.06) !important;
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: white;
}

.proof-row {
  align-items: center;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  justify-content: space-between;
  padding: 14px 0;
}

.proof-row span,
.info-card p,
.metric-card p,
.notice-card p,
.section-heading p {
  color: rgba(255, 255, 255, 0.68);
}

.proof-row strong {
  color: #ffb000;
  margin-left: 18px;
  text-align: right;
}

.section-heading {
  margin: 0 auto;
  max-width: 840px;
  text-align: center;
}

.section-heading h2 {
  font-size: clamp(2rem, 4vw, 3.4rem);
  font-weight: 850;
  letter-spacing: -0.04em;
  line-height: 1.05;
}

.section-heading p {
  font-size: 1.08rem;
  line-height: 1.8;
  margin-top: 18px;
}

.surface-section {
  background: #141414;
}

.step-number {
  color: #ffb000;
  font-size: 0.9rem;
  font-weight: 900;
  letter-spacing: 0.12em;
  margin-bottom: 18px;
}

.info-card h3,
.metric-card h3,
.notice-card h2 {
  color: white;
  font-weight: 800;
  margin-bottom: 12px;
}

.flow-text {
  color: #ffb000;
  font-size: clamp(1rem, 2vw, 1.35rem);
  font-weight: 750;
  line-height: 1.7;
  text-align: center;
}

.checklist-card :deep(.v-list-item-title) {
  color: white;
  font-weight: 700;
}

.checklist-card :deep(.v-list-item-subtitle) {
  color: rgba(255, 255, 255, 0.64);
}
</style>
