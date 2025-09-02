<template>
  <div class="p-4">
    <!-- Input for number with two-way binding -->
    <input
      v-model="localNumber"
      type="number"
      placeholder="Example: 12345678"
      class="w-full p-2 border rounded-md mb-4"
    />

    <!-- Button Group for GET and POST Actions -->
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
      // Local copy of the number for v-model binding
      localNumber: this.number,
    };
  },
  methods: {
    // Handle input update and emitting value
    updateNumber(value) {
      this.$emit('update:number', value);
      this.initValue();  // Call initValue after updating
    },

    // Handle button click for GET and POST
    handleFetch(method) {
      if (method === 'GET') {
        this.fetchGet();
      } else if (method === 'POST') {
        this.fetchPost();
      }
    },
  },
  watch: {
    // Watch for external changes to the 'number' prop and sync with localNumber
    number(newValue) {
      this.localNumber = newValue;
    },
  },
}
</script>

<style scoped>
/* Scoped styles for the component */
</style>