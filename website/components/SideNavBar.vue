<script setup lang="ts">
const { data } = await useAsyncData('nav', () => fetchContentNavigation(),
)

const navigation = computed(() => data.value?.[0].children)
</script>

<template>
  <div>
    <div v-for="nav in navigation" :key="nav._id" class="mb-6">
      <h4 class="font-semibold text-lg capitalize">
        {{ nav.title }}
      </h4>

      <div class="mt-2 text-sm flex flex-col space-y-1">
        <NuxtLink
          v-for="child in nav.children"
          :key="child.title"
          class="hover:underline inline-flex items-center"
          :to="child._path"
        >
          {{ child.title }}

          <span v-if="child.new" class="text-[0.65rem] px-1.5 ml-1 h-4 flex items-center rounded-full bg-blue-50 text-blue-400">New</span>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<style scoped>
.router-link-active {
  @apply text-blue-400;
}
</style>
