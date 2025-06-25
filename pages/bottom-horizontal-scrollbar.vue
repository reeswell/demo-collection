<template>
  <div class="page-container">
    <h1>Bottom Scrollbar Control Example</h1>
    <p>This example demonstrates how to create a bottom scrollbar to control horizontal scrolling of a page element.</p>

    <div class="scroll-container" ref="scrollBox">
      <div class="scroll-content">
        <div class="content-markers">
          <div v-for="marker in markers" :key="marker.position" class="marker"
            :style="{ left: marker.position + 'px' }">
            <div class="marker-label">{{ marker.label }}</div>
          </div>
        </div>

        <div class="position-indicator" ref="positionIndicator">
          Current Position: {{ currentPosition }}px
        </div>

        <div class="demo-content">
          <h3>Scrollable Content Area</h3>
          <p>This is a 1200px wide content area in a 400px container.</p>
          <p>You can control horizontal scrolling by:</p>
          <ul>
            <li>Scrolling directly in this area</li>
            <li>Using the custom scrollbar at the bottom</li>
            <li>Using the control buttons below</li>
          </ul>
          <p>The bottom scrollbar syncs perfectly with this area!</p>

          <div style="margin-top: 100px;">
            <h4>More Content Area</h4>
            <p>Continue scrolling horizontally to see more content...</p>
            <p>Here is some very long long long long long long long long text content</p>
          </div>
        </div>
      </div>
    </div>

    <div class="info-panel">
      <h3>Scroll Information:</h3>
      <p>scrollLeft: <span>{{ scrollInfo.scrollLeft }}</span>px</p>
      <p>Max scroll distance: <span>{{ scrollInfo.maxScroll }}</span>px</p>
      <p>Scroll percentage: <span>{{ scrollInfo.scrollPercent }}</span>%</p>
    </div>

    <div class="controls">
      <h3>Quick Controls:</h3>
      <button @click="scrollToPosition(0)">Scroll to Start</button>
      <button @click="scrollToPosition(600)">Scroll to 600</button>
      <button @click="scrollToPosition(1200)">Scroll to 1200</button>
      <button @click="scrollToPosition(1800)">Scroll to 1800</button>
      <button @click="scrollToEnd">Scroll to End</button>
    </div>

    <div class="bottom-scrollbar-container">
      <div class="bottom-scrollbar-label">Horizontal Control:</div>
      <div class="custom-scrollbar" ref="customScrollbar" @click="handleScrollbarClick">
        <div class="custom-scrollbar-thumb" ref="customScrollbarThumb" @mousedown="handleMouseDown"
          @touchstart="handleTouchStart" :style="thumbStyle"></div>
      </div>
      <div class="scroll-info">
        Position: {{ scrollInfo.scrollLeft }}px / {{ scrollInfo.maxScroll }}px ({{ scrollInfo.scrollPercent }}%)
      </div>
    </div>
  </div>
</template>

<script setup>
useHead({
  title: 'Bottom Scrollbar Control Example'
})

const scrollBox = ref(null)
const customScrollbar = ref(null)
const customScrollbarThumb = ref(null)
const positionIndicator = ref(null)

const markers = [
  { position: 0, label: 'Start' },
  { position: 600, label: '600px' },
  { position: 1200, label: '1200px' },
  { position: 1800, label: '1800px' },
  { position: 2400, label: '2400px' },
  { position: 3000, label: '3000px' }
]

const scrollInfo = reactive({
  scrollLeft: 0,
  maxScroll: 0,
  scrollPercent: 0
})

const thumbStyle = reactive({
  width: '30px',
  left: '0px'
})

const currentPosition = computed(() => Math.round(scrollInfo.scrollLeft))

let isDragging = false
let dragStartX = 0
let dragStartScrollLeft = 0

const calculateScrollbarValues = () => {
  if (!scrollBox.value || !customScrollbar.value) return {}

  const maxScroll = scrollBox.value.scrollWidth - scrollBox.value.clientWidth
  const scrollPercent = maxScroll > 0 ? (scrollBox.value.scrollLeft / maxScroll) * 100 : 0
  const thumbWidth = Math.max(30, (scrollBox.value.clientWidth / scrollBox.value.scrollWidth) * customScrollbar.value.offsetWidth)
  const thumbPosition = (scrollBox.value.scrollLeft / maxScroll) * (customScrollbar.value.offsetWidth - thumbWidth)

  return {
    maxScroll,
    scrollPercent,
    thumbWidth,
    thumbPosition: isNaN(thumbPosition) ? 0 : thumbPosition
  }
}

const updateCustomScrollbar = () => {
  const values = calculateScrollbarValues()

  thumbStyle.width = values.thumbWidth + 'px'
  thumbStyle.left = values.thumbPosition + 'px'

  scrollInfo.scrollLeft = Math.round(scrollBox.value.scrollLeft)
  scrollInfo.maxScroll = Math.round(values.maxScroll)
  scrollInfo.scrollPercent = Math.round(values.scrollPercent)
}

const handleScroll = () => {
  if (!isDragging) {
    updateCustomScrollbar()
  }
}

const handleScrollbarClick = (e) => {
  if (e.target === customScrollbarThumb.value) return

  const rect = customScrollbar.value.getBoundingClientRect()
  const clickX = e.clientX - rect.left
  const scrollbarWidth = customScrollbar.value.offsetWidth
  const thumbWidth = parseFloat(thumbStyle.width)

  const maxScroll = scrollBox.value.scrollWidth - scrollBox.value.clientWidth
  const targetScrollLeft = ((clickX - thumbWidth / 2) / (scrollbarWidth - thumbWidth)) * maxScroll

  scrollBox.value.scrollLeft = Math.max(0, Math.min(maxScroll, targetScrollLeft))
}

const handleMouseDown = (e) => {
  isDragging = true
  dragStartX = e.clientX
  dragStartScrollLeft = scrollBox.value.scrollLeft

  document.addEventListener('mousemove', handleMouseMove)
  document.addEventListener('mouseup', handleMouseUp)

  e.preventDefault()
}

const handleMouseMove = (e) => {
  if (!isDragging) return

  const deltaX = e.clientX - dragStartX
  const scrollbarWidth = customScrollbar.value.offsetWidth
  const thumbWidth = parseFloat(thumbStyle.width)
  const maxScroll = scrollBox.value.scrollWidth - scrollBox.value.clientWidth

  const scrollRatio = deltaX / (scrollbarWidth - thumbWidth)
  const newScrollLeft = dragStartScrollLeft + (scrollRatio * maxScroll)

  scrollBox.value.scrollLeft = Math.max(0, Math.min(maxScroll, newScrollLeft))
  updateCustomScrollbar()
}

const handleMouseUp = () => {
  isDragging = false
  document.removeEventListener('mousemove', handleMouseMove)
  document.removeEventListener('mouseup', handleMouseUp)
}

const handleTouchStart = (e) => {
  isDragging = true
  dragStartX = e.touches[0].clientX
  dragStartScrollLeft = scrollBox.value.scrollLeft
  e.preventDefault()
}

const handleTouchMove = (e) => {
  if (!isDragging) return
  handleMouseMove({ clientX: e.touches[0].clientX })
  e.preventDefault()
}

const handleTouchEnd = (e) => {
  if (isDragging) {
    handleMouseUp()
    e.preventDefault()
  }
}

const handleKeydown = (e) => {
  const step = 50
  switch (e.key) {
    case 'ArrowLeft':
      scrollBox.value.scrollLeft = Math.max(0, scrollBox.value.scrollLeft - step)
      e.preventDefault()
      break
    case 'ArrowRight':
      const maxScroll = scrollBox.value.scrollWidth - scrollBox.value.clientWidth
      scrollBox.value.scrollLeft = Math.min(maxScroll, scrollBox.value.scrollLeft + step)
      e.preventDefault()
      break
    case 'Home':
      scrollBox.value.scrollLeft = 0
      e.preventDefault()
      break
    case 'End':
      scrollBox.value.scrollLeft = scrollBox.value.scrollWidth - scrollBox.value.clientWidth
      e.preventDefault()
      break
  }
}

const scrollToPosition = (position) => {
  scrollBox.value.scrollLeft = position
}

const scrollToEnd = () => {
  scrollBox.value.scrollLeft = scrollBox.value.scrollWidth - scrollBox.value.clientWidth
}

onMounted(() => {
  scrollBox.value?.addEventListener('scroll', handleScroll)
  document.addEventListener('keydown', handleKeydown)
  document.addEventListener('touchmove', handleTouchMove)
  document.addEventListener('touchend', handleTouchEnd)
  window.addEventListener('resize', updateCustomScrollbar)

  updateCustomScrollbar()
  console.log('Bottom scrollbar controller initialized')
})

onUnmounted(() => {
  scrollBox.value?.removeEventListener('scroll', handleScroll)
  document.removeEventListener('keydown', handleKeydown)
  document.removeEventListener('touchmove', handleTouchMove)
  document.removeEventListener('touchend', handleTouchEnd)
  document.removeEventListener('mousemove', handleMouseMove)
  document.removeEventListener('mouseup', handleMouseUp)
  window.removeEventListener('resize', updateCustomScrollbar)
})
</script>

<style scoped>
.page-container {
  margin: 0;
  padding: 20px;
  font-family: Arial, sans-serif;
  padding-bottom: 60px;
}

.scroll-container {
  width: 1200px;
  height: 250px;
  border: 2px solid #333;
  overflow: auto;
  margin: 0 auto;
  padding: 10px;
  background-color: #f9f9f9;
}

.scroll-content {
  width: 3600px;
  height: 500px;
  background: linear-gradient(90deg, #ff6b6b, #4ecdc4, #45b7d1, #96ceb4, #feca57, #ff9ff3);
  padding: 20px;
  box-sizing: border-box;
  position: relative;
}

.content-markers {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 40px;
  display: flex;
  align-items: center;
}

.marker {
  position: absolute;
  height: 30px;
  width: 2px;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
}

.marker-label {
  position: absolute;
  left: 5px;
  background: rgba(255, 255, 255, 0.8);
  padding: 2px 6px;
  border-radius: 3px;
  font-size: 12px;
  white-space: nowrap;
}

.bottom-scrollbar-container {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 40px;
  background-color: #2c3e50;
  border-top: 2px solid #34495e;
  display: flex;
  align-items: center;
  padding: 0 20px;
  box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.1);
  z-index: 1000;
}

.bottom-scrollbar-label {
  color: white;
  margin-right: 15px;
  font-size: 14px;
  font-weight: bold;
  min-width: 120px;
}

.custom-scrollbar {
  flex: 1;
  height: 20px;
  background-color: #34495e;
  border-radius: 10px;
  position: relative;
  cursor: pointer;
  margin-right: 15px;
}

.custom-scrollbar-thumb {
  height: 100%;
  background: linear-gradient(90deg, #3498db, #2980b9);
  border-radius: 10px;
  position: absolute;
  top: 0;
  left: 0;
  cursor: grab;
  transition: background 0.2s ease;
  min-width: 30px;
}

.custom-scrollbar-thumb:hover {
  background: linear-gradient(90deg, #5dade2, #3498db);
}

.custom-scrollbar-thumb:active {
  cursor: grabbing;
  background: linear-gradient(90deg, #2980b9, #1f618d);
}

.scroll-info {
  color: white;
  font-size: 12px;
  min-width: 150px;
}

.info-panel {
  margin: 20px;
  padding: 15px;
  background-color: #ecf0f1;
  border-radius: 8px;
  border-left: 4px solid #3498db;
}

.controls {
  margin: 20px;
}

button {
  margin: 5px;
  padding: 8px 16px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

button:hover {
  background-color: #2980b9;
}

.demo-content {
  margin-top: 60px;
}

.position-indicator {
  position: absolute;
  top: 60px;
  left: 20px;
  background: rgba(231, 76, 60, 0.9);
  color: white;
  padding: 10px;
  border-radius: 5px;
  font-weight: bold;
}
</style>
