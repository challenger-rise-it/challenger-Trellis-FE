<template>
  <div class="min-h-screen flex flex-col items-center justify-center bg-gray-100 p-4">
    <div class="bg-white p-6 rounded-lg shadow-md w-full max-w-md">
      <p class="text-2xl font-bold text-center mb-4">Convert Numbers to English Words</p>

      <NumberInput
        :number="number"
        @update:number="val => { number = val; initValue() }"
        :initValue="initValue"
        :fetchGet="fetchGet"
        :fetchPost="fetchPost"
      />

      <ResponseDisplay
        :result="result"
        :error="error"
        :loading="loading"
        :responseType="responseType"
      />
    </div>
  </div>
</template>

<script setup>
import axios from 'axios'
import { ref } from 'vue'
import NumberInput from './components/NumberInput.vue'
import ResponseDisplay from './components/ResponseDisplay.vue'

// ---- State ----
const number = ref('')
const result = ref('Result will be displayed here')
const loading = ref(false)
const error = ref('')
const responseType = ref('')

// ---- Axios instance ----
const api = axios.create({
  baseURL: import.meta.env.VITE_APP_API_BASE_URL,
  // You can add headers here if needed
})

// Track the current request to cancel stale ones
let controller = null

// ---- Helpers ----
const initValue = () => {
  error.value = ''
  result.value = 'Result will be displayed here'
}

const setLoadingState = (method) => {
  responseType.value = method
  loading.value = method === 'POST'
  error.value = ''
  result.value = method === 'POST' ? 'Waiting for response...' : ''
}

const clearLoadingState = () => {
  loading.value = false
}

// ---- Core fetcher ----
const fetchData = async (method) => {
  // Guard: no empty input
  if (number.value === '' || number.value === null || number.value === undefined) {
    error.value = 'Please enter a number first'
    result.value = ''
    return
  }

  // Cancel any in-flight request
  if (controller) controller.abort()
  controller = new AbortController()

  setLoadingState(method)

  try {
    let response
    if (method === 'GET') {
      response = await api.get(`/num_to_english/`, {
        params: { number: number.value },
        signal: controller.signal,
      })
    } else {
      response = await api.post(`/num_to_english/`, { number: number.value }, { signal: controller.signal })
    }

    result.value = response?.data?.num_in_english ?? 'No result returned'
    error.value = ''
  } catch (err) {
    // Ignore abort errors as they’re expected when typing fast / switching methods
    if (axios.isCancel?.(err) || err?.name === 'CanceledError' || err?.code === 'ERR_CANCELED') return

    // Prefer server message, else generic
    error.value =
      err?.response?.data?.message ||
      err?.response?.data?.error ||
      err?.message ||
      'An error occurred'
    result.value = ''
  } finally {
    clearLoadingState()
  }
}

// ---- Public methods (kept same signature) ----
const fetchGet = () => fetchData('GET')
const fetchPost = () => fetchData('POST')
</script>

<style scoped>
/* Add any local tweaks here if needed */
</style>
