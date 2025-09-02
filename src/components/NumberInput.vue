<template>
  <div>
    <input
      :value="number"
      @input="updateNumber($event.target.value)"
      type="number"
      placeholder="Example: 12345678"
      class="w-full p-2 border rounded-md mb-2"
    />
    <div class="flex justify-between gap-4 mb-2">
      <button @click="fetchGet" class="bg-green-500 text-white p-2 rounded-md w-1/2">GET</button>
      <button @click="fetchPost" class="bg-blue-500 text-white p-2 rounded-md w-1/2">POST</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    number: String,
    initValue: Function,
    fetchGet: Function,
    fetchPost: Function,
  },
  methods: {
    updateNumber(value) {
      this.$emit('update:number', value)
      this.initValue()
    },
  },
}
</script>

<style scoped></style>
