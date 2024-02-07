<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import TopBar from '@tibnor/openbridge-webcomponents-vue/components/top-bar/ObcTopBar'
import '@tibnor/openbridge-webcomponents/dist/components/navigation-item/navigation-item.js'
import BrillianceMenu from '@tibnor/openbridge-webcomponents-vue/components/brilliance-menu/ObcBrillianceMenu'
import ObcAlertTopbarElement from '@tibnor/openbridge-webcomponents-vue/components/alert-topbar-element/ObcAlertTopbarElement'

import '@tibnor/openbridge-webcomponents/dist/icons/icon-14-alarm-unack'

import { AlertType } from '@tibnor/openbridge-webcomponents/dist/types'

if (import.meta.env.PROD) {
  //@ts-expect-error TS2306
  import('@tibnor/openbridge-webcomponents/dist/icons/index.js')
}

const date = ref(new Date().toISOString())

onMounted(() => {
  // update date every second
  setInterval(() => {
    date.value = new Date().toISOString()
  }, 1000)

  //@ts-expect-error TS2306
  import('@tibnor/openbridge-webcomponents/dist/icons/index.js')
})

const palette = ref('day')

watch(
  palette,
  (value) => {
    // set data-obc-theme attribute on html element
    document.documentElement.setAttribute('data-obc-theme', value)
  },
  { immediate: true }
)

function onPaletteChange(event: CustomEvent) {
  palette.value = event.detail.value;
}

const showBrilliance = ref(false)

function toggleBrilliance() {
  showBrilliance.value = !showBrilliance.value
}
</script>

<!-- eslint-disable vue/no-deprecated-slot-attribute -->
<template>
  <header>
    <TopBar
      app-title="Demo"
      page-name="Vue"
      :date="date"
      @dimming-button-clicked="toggleBrilliance"
      show-apps-button
      show-dimming-button
      show-clock
    >
      <template #alerts>
        <ObcAlertTopbarElement
          style="width: 500px"
          :n-alerts="0"
          :max-width="500"
          :alert-type="AlertType.None"
          :show-ack="false"
          :alert-muted="true"
        >
        </ObcAlertTopbarElement>
      </template>
    </TopBar>
  </header>
  <main>
    <div class="content">
      <BrillianceMenu
        :palette="palette"
        @palette-changed="onPaletteChange"
        show-auto-brightness
        class="brilliance"
        v-if="showBrilliance"
      >
      </BrillianceMenu>
    </div>
  </main>
</template>

<style scoped>
header {
  position: absolute;
  z-index: 1;
  top: 0;
  left: 0;
  right: 0;
}

.content {
  isolation: isolate;
  position: absolute;
  top: 48px;
  bottom: 0;
  left: 0;
  right: 0;
}

  .brilliance {
    position: absolute;
    top: 4px;
    right: 48px;
  }

</style>
