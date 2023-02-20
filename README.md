# vue3-fixed-size-accordion

Easy to use, lightweight accordion panel for Vue 3, with stable height in opened and closed states.


[screencast-localhost_5173-2022.11.26-16_26_23.webm](https://user-images.githubusercontent.com/12416010/204093958-582c6f45-f780-4518-9ce6-cae9dcd80c37.webm?width="100")

## Installation
```shell
# with npm
npm install vue-fixed-size-accordion
```
```shell
# with yarn
yarn add vue-fixed-size-accordion
```

## Slots
```javascript
#topBar // Here you can place list header with hidden filters block, as in video example

#bottomBar // Main content. Here you can place list items.
```

## 

## Getting Started
### Import component
```javascript
import FixedSizeAccordion from 'vue-fixed-size-accordion'

export default {
  ...
  components: {
    'fixed-size-accordion': FixedSizeAccordion
  }
  ...
}
```

## How to use
```vue
    <fixed-size-accordion :open="isOpened" :animation-duration="500">
      <template v-slot:topBar>
        <div>
          <div class="visible-header">
            <h3>This is the list header, for example</h3>
            <button @click="isOpened = !isOpened">
              {{ isOpened ? 'Close' : 'Open' }}
            </button>
          </div>
          <div v-if="isOpened">
            <p>This is hidden block. There are could be list filters, info block or any what you want.</p>
          </div>
        </div>
      </template>
      
      <template v-slot:bottomBar>
        <div>
          <div v-for="number in 5" class="item" :key="number">
            Item: {{ number }}
          </div>
        </div>
      </template>
    </fixed-size-accordion>
```
__NOTE: topBar slot should have one child__

## Props
| NAME                    | TYPE      | DEFAULT      |  DESCRIPTION                                                                                   |
|-------------------------|-----------|--------------|------------------------------------------------------------------------------------------------|   
| animationDuration? (ms) | Number    |    0         | Set sliding animation duration. Animation not applying, if value **0**                         |
| topBarId?               | String    | fsaTopBar    | **topBar** slot wrapper **id**                                                                 |  
| bottomBarId?            | String    | fsaBottomBar | **bottomBar** slot wrapper **id**                                                              |
| bottomBarContentId?     | String    | " "          | By default bottom bar height calculations performing on block with **bottomBarId**. You can add your component with **id** to bottom bar and pass that **id** as a prop - calculations will perform on that component |

