<script setup lang="ts">
import { ref, onMounted } from 'vue'
import type { BuilderContent } from '@builder.io/sdk-vue';
import { Content, fetchOneEntry, isPreviewing, track } from '@builder.io/sdk-vue';

const content = ref<BuilderContent | null>(null);
const apiKey = '7be8e770719149f8b0e7ff363c39fbe7'; // '92ca7814a8024706bba0c71abac33e32';
const canShowContent = ref(false);
const model = 'page';

defineProps({
  msg: String,
})

onMounted(async () => {
  content.value = await fetchOneEntry({
    model,
    apiKey,
    userAttributes: {
      urlPath: window.location.pathname,
    },
  });
  console.log(`content ${content.value} for path ${window.location.pathname}`);

  // Track a page view
  // NOTE: Comment out to test if conversions are tracked without impression
  await track({ type: 'impression', apiKey });

  // Track a conversion  
  await track({ type: 'conversion', amount: 90, apiKey });
  canShowContent.value = content.value ? true : isPreviewing();
});

const count = ref(0)
</script>

<template>
  <h1>{{ msg }}</h1>

  <Content
    v-if="canShowContent"
    :model="model"
    :content="content"
    :api-key="apiKey"
  />
<div v-else>Content not Found</div>
 
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
