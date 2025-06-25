<script setup lang="ts">

</script>

<template>
  <header class="bg-gray-700 w-full">
    <div class="container mx-auto flex h-full max-w-7xl items-center justify-between gap-2 px-2 mb-5">
      <!-- left -->
      <div class="basis-auto items-center gap-2">
        <NuxtLink to="/" class="flex items-end gap-1.5 font-bold text-xl text-gray-900 dark:text-white">
          <Logo class="w-auto h-6" />
        </NuxtLink>
      </div>

      <!-- right -->
      <div class="basis-auto">
        <DarkToggle />
      </div>
    </div>
  </header>
</template>
