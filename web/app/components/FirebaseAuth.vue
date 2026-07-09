<script setup lang="ts">
import {
  getAuth,
  signInWithEmailAndPassword,
  sendPasswordResetEmail,
} from 'firebase/auth'
import { mdiEmail, mdiLockOutline, mdiTicketConfirmationOutline } from '@mdi/js'
import type { User as FirebaseUser } from 'firebase/auth'
import { ErrorMessages } from '~/utils/errors'

const props = withDefaults(
  defineProps<{
    to?: string
  }>(),
  { to: '/' },
)

const router = useRouter()
const config = useRuntimeConfig()
const authStore = useAuthStore()
const notificationsStore = useNotificationsStore()
const appStore = useAppStore()

const loading = ref(false)
const mode = ref<'login' | 'invite' | 'reset'>('login')
const resetEmailSent = ref(false)
const email = ref('')
const password = ref('')
const inviteCode = ref('')
const generalError = ref('')
const errorMessages = ref(new ErrorMessages())

function clearErrors() {
  errorMessages.value = new ErrorMessages()
  generalError.value = ''
}

function validateEmail(): boolean {
  clearErrors()

  if (!email.value.trim()) {
    errorMessages.value.add('email', 'Please provide an email address')
    return false
  }

  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!emailRegex.test(email.value.trim())) {
    errorMessages.value.add('email', 'Please enter a valid email address')
    return false
  }

  return true
}

function validateLoginForm(): boolean {
  clearErrors()

  let valid = true

  if (!email.value.trim()) {
    errorMessages.value.add('email', 'Please provide an email address')
    valid = false
  } else {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    if (!emailRegex.test(email.value.trim())) {
      errorMessages.value.add('email', 'Please enter a valid email address')
      valid = false
    }
  }

  if (!password.value) {
    errorMessages.value.add('password', 'Please enter your password')
    valid = false
  }

  return valid
}

function validateInviteForm(): boolean {
  const validLogin = validateLoginForm()
  let valid = validLogin

  if (!inviteCode.value.trim()) {
    errorMessages.value.add('invite_code', 'Please enter your invite code')
    valid = false
  }

  return valid
}

async function submitLogin() {
  if (!validateLoginForm()) return

  loading.value = true
  try {
    const auth = getAuth()
    const result = await signInWithEmailAndPassword(
      auth,
      email.value.trim(),
      password.value,
    )

    onSuccess(result.user)
  } catch (error: unknown) {
    handleError(error)
  } finally {
    loading.value = false
  }
}

async function submitInviteSignup() {
  if (!validateInviteForm()) return

  loading.value = true
  try {
    await $fetch('/v1/auth/invite-signup', {
      baseURL: config.public.apiBaseUrl,
      method: 'POST',
      body: {
        email: email.value.trim(),
        password: password.value,
        invite_code: inviteCode.value.trim(),
      },
    })

    const auth = getAuth()
    const result = await signInWithEmailAndPassword(
      auth,
      email.value.trim(),
      password.value,
    )

    notificationsStore.addNotification({
      message: 'Invited account created successfully.',
      type: 'success',
    })

    onSuccess(result.user)
  } catch (error: unknown) {
    handleError(error)
  } finally {
    loading.value = false
  }
}

async function submitPasswordReset() {
  if (!validateEmail()) return

  loading.value = true
  try {
    const auth = getAuth()
    await sendPasswordResetEmail(auth, email.value.trim())
    resetEmailSent.value = true
  } catch (error: unknown) {
    handleError(error)
  } finally {
    loading.value = false
  }
}

function switchMode(nextMode: 'login' | 'invite' | 'reset') {
  clearErrors()
  resetEmailSent.value = false
  mode.value = nextMode
}

function onSuccess(user: FirebaseUser) {
  notificationsStore.addNotification({
    message: 'Login successful.',
    type: 'success',
  })

  authStore.onAuthStateChanged(user)
  router.push({ path: props.to })
}

function handleError(error: unknown) {
  clearErrors()

  const err = error as {
    code?: string
    message?: string
    data?: {
      message?: string
      data?: Record<string, string[]>
    }
  }

  if (err.data?.data) {
    for (const [field, messages] of Object.entries(err.data.data)) {
      for (const message of messages) {
        errorMessages.value.add(field, message)
      }
    }
    return
  }

  const code = err.code || ''

  switch (code) {
    case 'auth/wrong-password':
      errorMessages.value.add('password', 'Incorrect password')
      break
    case 'auth/invalid-credential':
      errorMessages.value.add('email', 'Invalid email or password')
      errorMessages.value.add('password', 'Invalid email or password')
      break
    case 'auth/user-not-found':
      errorMessages.value.add(
        'email',
        'No account found with this email address',
      )
      break
    case 'auth/invalid-email':
      errorMessages.value.add('email', 'Please enter a valid email address')
      break
    case 'auth/email-already-in-use':
      errorMessages.value.add(
        'email',
        'An account already exists with this email',
      )
      break
    case 'auth/weak-password':
      errorMessages.value.add(
        'password',
        'Password should be at least 6 characters',
      )
      break
    case 'auth/user-disabled':
      errorMessages.value.add('email', 'This account has been disabled')
      break
    case 'auth/too-many-requests':
      generalError.value = 'Too many failed attempts. Please try again later'
      break
    case 'auth/network-request-failed':
      generalError.value =
        'Unable to connect to the server. Please check your internet connection'
      break
    default:
      if (err.data?.message === 'Forbidden') {
        errorMessages.value.add('invite_code', 'Invalid invite code')
      } else {
        generalError.value =
          err.data?.message || err.message || 'An unexpected error occurred'
      }
  }
}
</script>

<template>
  <div>
    <VAlert type="info" variant="tonal" class="mb-4">
      Use email/password for existing DeliSMS users. New accounts require an
      invite code from the gateway owner.
    </VAlert>

    <VForm
      v-if="mode === 'login'"
      class="mt-4"
      @submit.prevent="submitLogin"
    >
      <VTextField
        v-model="email"
        label="Email Address"
        color="primary"
        type="email"
        variant="outlined"
        density="comfortable"
        class="mb-2"
        :prepend-inner-icon="mdiEmail"
        :error="errorMessages.has('email')"
        :error-messages="errorMessages.get('email')"
      />

      <VTextField
        v-model="password"
        label="Password"
        type="password"
        color="primary"
        variant="outlined"
        density="comfortable"
        class="mb-2"
        :prepend-inner-icon="mdiLockOutline"
        :error="errorMessages.has('password')"
        :error-messages="errorMessages.get('password')"
      />

      <VAlert v-if="generalError" type="error" density="compact" class="mb-3">
        {{ generalError }}
      </VAlert>

      <VBtn
        block
        size="large"
        color="primary"
        type="submit"
        :loading="loading"
      >
        Sign In
      </VBtn>

      <div class="d-flex justify-space-between mt-3">
        <VBtn
          variant="plain"
          size="small"
          color="primary"
          class="px-0"
          @click="switchMode('reset')"
        >
          Forgot Password?
        </VBtn>

        <VBtn
          variant="plain"
          size="small"
          color="primary"
          class="px-0"
          @click="switchMode('invite')"
        >
          Create account with invite code
        </VBtn>
      </div>
    </VForm>

    <VForm
      v-if="mode === 'invite'"
      class="mt-4"
      @submit.prevent="submitInviteSignup"
    >
      <VTextField
        v-model="email"
        label="Email Address"
        color="primary"
        type="email"
        variant="outlined"
        density="comfortable"
        class="mb-2"
        :prepend-inner-icon="mdiEmail"
        :error="errorMessages.has('email')"
        :error-messages="errorMessages.get('email')"
      />

      <VTextField
        v-model="password"
        label="Password"
        type="password"
        color="primary"
        variant="outlined"
        density="comfortable"
        class="mb-2"
        :prepend-inner-icon="mdiLockOutline"
        :error="errorMessages.has('password')"
        :error-messages="errorMessages.get('password')"
      />

      <VTextField
        v-model="inviteCode"
        label="Invite Code"
        type="password"
        color="primary"
        variant="outlined"
        density="comfortable"
        class="mb-2"
        :prepend-inner-icon="mdiTicketConfirmationOutline"
        :error="errorMessages.has('invite_code')"
        :error-messages="errorMessages.get('invite_code')"
      />

      <VAlert v-if="generalError" type="error" density="compact" class="mb-3">
        {{ generalError }}
      </VAlert>

      <VBtn
        block
        size="large"
        color="primary"
        type="submit"
        :loading="loading"
      >
        Create Invited Account
      </VBtn>

      <VBtn
        block
        variant="text"
        size="small"
        color="warning"
        class="mt-2"
        @click="switchMode('login')"
      >
        Back to Sign In
      </VBtn>
    </VForm>

    <VForm
      v-if="mode === 'reset'"
      class="mt-4"
      @submit.prevent="submitPasswordReset"
    >
      <template v-if="!resetEmailSent">
        <p class="text-body-medium text-medium-emphasis mb-4">
          Enter your email address to reset your password.
        </p>

        <VTextField
          v-model="email"
          label="Email Address"
          color="primary"
          type="email"
          variant="outlined"
          density="comfortable"
          class="mb-2"
          :prepend-inner-icon="mdiEmail"
          :error="errorMessages.has('email')"
          :error-messages="errorMessages.get('email')"
        />

        <VAlert
          v-if="generalError"
          type="error"
          density="compact"
          class="mb-3"
        >
          {{ generalError }}
        </VAlert>

        <VBtn
          block
          size="large"
          color="primary"
          type="submit"
          :loading="loading"
        >
          Send Reset Link
        </VBtn>
      </template>

      <template v-else>
        <VAlert type="success" density="compact" class="mb-3">
          Check your email for password reset instructions.
        </VAlert>
      </template>

      <VBtn
        block
        variant="text"
        size="small"
        color="warning"
        class="mt-2"
        @click="switchMode('login')"
      >
        Back to Sign In
      </VBtn>
    </VForm>

    <p class="text-body-small text-medium-emphasis mt-4">
      By continuing, you are accepting the DeliSMS lab
      <a
        :href="appStore.appData.url + '/terms-and-conditions'"
        class="text-decoration-none"
      >
        Terms
      </a>
      and
      <a
        :href="appStore.appData.url + '/privacy-policy'"
        class="text-decoration-none"
      >
        Privacy Notice.</a
      >
    </p>
  </div>
</template>
