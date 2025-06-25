<script setup lang="ts">
const { value } = defineProps<{
  value?: boolean,
}>()

const menuItems = [
  {
    title: 'Vue Horizontal Scrollbar Example',
    path: '/vue-horizontal-scrollbar',
    description: 'Vue 水平滚动条示例',
    icon: '🎯'
  },
  {
    title: 'Bottom Scrollbar Control Example',
    path: '/bottom-horizontal-scrollbar',
    description: '底部滚动条控制示例',
    icon: '📊'
  }
]

const githubUrl = 'https://github.com/reeswell/demo-collection'
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 py-12 px-4">
    <div class="max-w-4xl mx-auto">
      <!-- 头部标题 -->
      <div class="text-center mb-12">
        <h1 class="text-4xl font-bold text-gray-800 mb-4">
          🚀 Demo Collection
        </h1>
        <p class="text-gray-600 text-lg">
          探索各种前端组件和功能示例
        </p>
      </div>

      <!-- 菜单列表 -->
      <div class="grid gap-6 md:grid-cols-2 lg:grid-cols-2 mb-12">
        <NuxtLink v-for="item in menuItems" :key="item.path" :to="item.path"
          class="group block p-6 bg-white rounded-xl shadow-md hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 border border-gray-100 hover:border-blue-200">
          <div class="flex items-start space-x-4">
            <div class="text-3xl group-hover:scale-110 transition-transform duration-300">
              {{ item.icon }}
            </div>
            <div class="flex-1">
              <h3
                class="text-xl font-semibold text-gray-800 group-hover:text-blue-600 transition-colors duration-300 mb-2">
                {{ item.title }}
              </h3>
              <p class="text-gray-600 text-sm">
                {{ item.description }}
              </p>
              <div class="mt-3 flex items-center text-blue-500 text-sm font-medium">
                <span>查看示例</span>
                <svg class="w-4 h-4 ml-1 group-hover:translate-x-1 transition-transform duration-300" fill="none"
                  stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                </svg>
              </div>
            </div>
          </div>
        </NuxtLink>
      </div>

      <!-- GitHub 链接 -->
      <div class="text-center">
        <div class="inline-flex items-center justify-center p-6 bg-white rounded-xl shadow-md border border-gray-100">
          <div class="flex items-center space-x-3">
            <svg class="w-8 h-8 text-gray-800" fill="currentColor" viewBox="0 0 24 24">
              <path
                d="M12 0C5.374 0 0 5.373 0 12 0 17.302 3.438 21.8 8.207 23.387c.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23A11.509 11.509 0 0112 5.803c1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576C20.566 21.797 24 17.3 24 12c0-6.627-5.373-12-12-12z" />
            </svg>
            <div class="text-left">
              <p class="text-sm text-gray-600 mb-1">项目源码</p>
              <a :href="githubUrl" target="_blank" rel="noopener noreferrer"
                class="text-blue-600 hover:text-blue-800 font-medium transition-colors duration-300 flex items-center">
                <span>GitHub Repository</span>
                <svg class="w-4 h-4 ml-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"></path>
                </svg>
              </a>
            </div>
          </div>
        </div>
      </div>

      <!-- 底部信息 -->
      <div class="text-center mt-12 text-gray-500 text-sm">
        <p>Built with ❤️ using Nuxt.js & Tailwind CSS</p>
      </div>
    </div>
  </div>
</template>
