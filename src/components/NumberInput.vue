<template>
  <div class="p-4">
    <input
      v-model="localNumber"
      type="number"
      placeholder="Example: 12345678"
      class="w-full p-2 border rounded-md mb-4"
    />

    <div class="flex justify-between gap-4 mb-2">
      <button @click="handleFetch('GET')" class="bg-green-500 text-white p-2 rounded-md w-1/2">GET</button>
      <button @click="handleFetch('POST')" class="bg-blue-500 text-white p-2 rounded-md w-1/2">POST</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    number: {
      type: String,
      required: true,
    },
    initValue: {
      type: Function,
      required: true,
    },
    fetchGet: {
      type: Function,
      required: true,
    },
    fetchPost: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      localNumber: this.number,
    };
  },
  methods: {
    updateNumber(value) {
      this.$emit('update:number', value);
      this.initValue(); 
    },

    handleFetch(method) {
      if (method === 'GET') {
        this.fetchGet();
      } else if (method === 'POST') {
        this.fetchPost();
      }
    },
  },
  watch: {
    number(newValue) {
      this.localNumber = newValue;
    },
  },
}
</script>

<style scoped>
</style>