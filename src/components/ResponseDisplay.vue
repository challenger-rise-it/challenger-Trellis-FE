<template>
  <div class="flex justify-between align-center bg-gray-100 p-2 rounded-md mb-2 h-24">
    <div>
      <p class="font-bold">{{ responseType }} Response</p>
      <p :class="{ 'text-green-600': result, 'text-red-500': error }">
        {{ result || error }}
      </p>
    </div>
    <div v-if="loading" class="flex justify-center mt-2">
      <span
        class="animate-spin border-4 border-blue-500 border-t-transparent rounded-full w-6 h-6 inline-block"
      ></span>
    </div>
  </div>
</template>



<script>
export default {
  props: {
    result: String,
    error: String,
    loading: Boolean,
    responseType: String,
  },
}
</script>

<style scoped></style>
