<script setup>
import TheWelcome from "@/components/TheWelcome.vue";
</script>

<script>
export default {
   mounted() {
      document.title = "JSON Paradox | Home";
   },
};
</script>

<template>
   <main>
      <TheWelcome />
   </main>
</template>
