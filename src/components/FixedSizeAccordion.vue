<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'

const props = defineProps<{
  open: boolean,
  animated?: boolean,
}>()

let observerInstance: ResizeObserver | null = null

const heightCalculation = (topBar: HTMLElement, bottomBar: HTMLElement): ResizeObserver => {
  const topBarContent = topBar.firstElementChild as HTMLElement | null

  if (!topBarContent) throw Error('Top bar should have have one child element')

  const topBarInitialHeight: number = topBarContent.offsetHeight
  const scrollBlockInitialHeight: number = bottomBar.offsetHeight

  let finishedTopBarBlockHeight = topBarInitialHeight
  let finishedContentBlockHeight = scrollBlockInitialHeight

  topBar.style.height = `${ topBarInitialHeight }px`
  bottomBar.style.height = `${ scrollBlockInitialHeight }px`

  const observer = new ResizeObserver((entries: ResizeObserverEntry[]) => {
    if (props.open) {
      finishedTopBarBlockHeight = entries[0].borderBoxSize[0].blockSize
      finishedContentBlockHeight = scrollBlockInitialHeight - (finishedTopBarBlockHeight - topBarInitialHeight)

      topBar.style.height = `${ Math.round(finishedTopBarBlockHeight) }px`
      bottomBar.style.height = `${ Math.round(finishedContentBlockHeight) }px`
    } else {
      const x = entries[0].borderBoxSize[0].blockSize
      const y = finishedTopBarBlockHeight - x

      topBar.style.height = `${ Math.round(x) }px`
      bottomBar.style.height = `${ Math.round(finishedContentBlockHeight + y) }px`
    }
  })

  observer.observe(topBarContent)

  return observer
}

onMounted(() => {
  const topBar: HTMLElement | null = document.getElementById('fsaTopBar')
  const bottomBar: HTMLElement | null = document.getElementById('fsaBottomBar')

  if (topBar && bottomBar) {
    observerInstance = heightCalculation(topBar, bottomBar)
  }
})

onUnmounted(() => {
  observerInstance?.disconnect()
})
</script>

<template>
  <div>
    <div id="fsaTopBar">
      <slot name="topBar" />
    </div>
    <div id="fsaBottomBar">
      <slot name="bottomBar" />
    </div>
  </div>
</template>

<style scoped>
#fsaTopBar {
  overflow: hidden;
  transition: height 500ms ease;
}

#fsaBottomBar {
  overflow-y: auto;
  transition: height 500ms ease;
}
</style>