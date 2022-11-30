<template>
  <div class="container">
    <fixed-size-accordion :open="isOpened" animated class="slide-block">
      <template v-slot:topBar>
        <div class="header">
          <div>
            <div class="visible-header">
              <h3>This is the list header, for example</h3>
              <button @click="isOpened = !isOpened">
                {{ isOpened ? 'Close' : 'Open' }}
              </button>
            </div>
            <div v-if="isOpened">
              <p>This is hidden block. There are could be list filters, info block or any what you want.</p>
              <p>Can be any height</p>
              <p>
                Whole accordion height do not changes when this block opened or closed, and all list elements are visible
              </p>
            </div>
          </div>
        </div>
      </template>
      <template v-slot:bottomBar>
        <div id="listWrapper">
          <div v-for="number in 5" class="item" :key="number">
            Item: {{ number }}
          </div>
        </div>
      </template>
    </fixed-size-accordion>
  </div>

</template>

<script setup lang="ts">
import { ref } from 'vue'
import FixedSizeAccordion from '../lib/index.ts'

const isOpened = ref<boolean>(false)

</script>

<style scoped>
:deep(#fsaTopBar) {
  background: #213547;
  color: #f9f9f9;
}
.visible-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.container {
  width: 500px;
}
button {
  margin-bottom: 1rem;
}
.slide-block {
  border: solid 1px;
}
.header {
  padding: 1rem;
}
.item {
  padding: 2rem;
  font-size: 2rem;
}
</style>
