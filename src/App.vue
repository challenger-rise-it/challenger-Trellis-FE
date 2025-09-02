<template>
  <div class="min-h-screen flex flex-col items-center justify-center bg-gray-100 p-4">
    <div class="bg-white p-6 rounded-lg shadow-md w-full max-w-md">
      <p class="text-2xl font-bold text-center mb-4">Convert Numbers to English Words</p>
      <NumberInput
        :number="number"
        @update:number="number = $event"
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

<script>
import axios from 'axios'
import { ref } from 'vue'
import NumberInput from './components/NumberInput.vue'
import ResponseDisplay from './components/ResponseDisplay.vue'

export default {
  components: {
    NumberInput,
    ResponseDisplay,
  },
  setup() {
    const baseUrl = import.meta.env.VITE_APP_API_BASE_URL

    const number = ref('')
    const result = ref('Result will be displayed here')
    const loading = ref(false)
    const error = ref('')
    const responseType = ref('')

    const initValue = () => {
      error.value = ''
      result.value = 'Result will be displayed here'
    }

    const fetchGet = async () => {
      await fetchData('GET')
    }

    const fetchPost = async () => {
      await fetchData('POST')
    }

    const fetchData = async (method) => {
      loading.value = method === 'POST'
      result.value = method === 'POST' ? 'Waiting for response...' : ''
      error.value = ''
      responseType.value = method

      try {
        const response =
          method === 'GET'
            ? await axios.get(`${baseUrl}/num_to_english/?number=${number.value}`)
            : await axios.post(`${baseUrl}/num_to_english/`, { number: number.value })
        result.value = response.data.num_in_english
      } catch (err) {
        error.value = err.response?.data?.message || 'An error occurred'
        result.value = ''
      } finally {
        loading.value = false
      }
    }

    return {
      number,
      result,
      loading,
      error,
      responseType,
      initValue,
      fetchGet,
      fetchPost,
    }
  },
}
</script>

<style scoped></style>
