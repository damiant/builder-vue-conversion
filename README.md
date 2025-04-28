# Vue 3 + Vite

This sample project calls to track impressions and conversions in the `HelloWorld.vue` component:
```typescript
  await track({ type: 'impression', apiKey })
  await track({ type: 'conversion', amount: 90, apiKey });
  ```