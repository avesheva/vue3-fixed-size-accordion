<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'

export interface IProps {
  animationDuration?: number,
  topBarId?: string,
  bottomBarId?: string,
  bottomBarContentId?: string,
}

const props = withDefaults(defineProps<IProps>(), {
  animationDuration: 0,
  topBarId: 'fsaTopBar',
  bottomBarId: 'fsaBottomBar',
  bottomBarContentId: '',
})

let observerInstance: ResizeObserver | null = null

const heightCalculation = (topBar: HTMLElement, bottomBar: HTMLElement): ResizeObserver => {
  const topBarContent = topBar.firstElementChild as HTMLElement | null

  if (!topBarContent) throw Error('Top bar should have have one child element')

  const topBarInitialHeight: number = topBarContent.offsetHeight
  const scrollBlockInitialHeight: number = bottomBar.offsetHeight

  const finishedTopBarBlockHeight = topBarInitialHeight
  const finishedContentBlockHeight = scrollBlockInitialHeight

  topBar.style.height = `${ topBarInitialHeight }px`
  bottomBar.style.height = `${ scrollBlockInitialHeight }px`

  const observer = new ResizeObserver((entries: ResizeObserverEntry[]) => {
    const x = entries[0].borderBoxSize[0].blockSize
    const y = finishedTopBarBlockHeight - x

    topBar.style.height = `${ Math.round(x) }px`
    bottomBar.style.height = `${ Math.round(finishedContentBlockHeight + y) }px`
  })

  observer.observe(topBarContent)

  return observer
}

onMounted(() => {
  const topBar: HTMLElement | null = document.getElementById(props.topBarId)
  const bottomBar: HTMLElement | null = document.getElementById(props.bottomBarContentId || props.bottomBarId)

  if (topBar && bottomBar) {
    topBar.style.transition = `height ${ props.animationDuration }ms ease`
    bottomBar.style.transition = `height ${ props.animationDuration }ms ease`

    observerInstance = heightCalculation(topBar, bottomBar)
  }
})

onUnmounted(() => {
  observerInstance?.disconnect()
})
</script>

<template>
  <div>
    <div
      :id="topBarId"
      style="overflow: hidden"
    >
      <slot name="topBar" />
    </div>
    <div
      :id="bottomBarId"
      style="overflow-y: auto"
    >
      <slot name="bottomBar" />
    </div>
  </div>
</template>
