<template>
  <div class="space-y-6">
    <div
      v-for="([prop, value]) in fontSizes"
      :key="prop"
    >
      <p
        class="mb-2 leading-none text-gray-900 dark:text-gray-500"
        :style="getFontSizeValue(value)"
      >
        {{ data.typographyExample }}
      </p>
      <CanvasBlockLabel
        :label="`text-${prop}`"
        :value="getFontSizeString(value)"
      />
    </div>
  </div>
</template>

<script>
import { remToPx } from '@/utils'
import CanvasBlockLabel from '../CanvasBlockLabel'

export default {
  components: {
    CanvasBlockLabel
  },
  props: {
    data: {
      type: Object,
      required: true
    },
    config: {
      type: Object,
      required: true
    }
  },
  computed: {
    fontSizes () {
      return Object.entries(this.data.fontSize).sort((a, b) => {
        if (remToPx(a[1], this.config) > remToPx(b[1], this.config)) {
          return 1
        }

        if (remToPx(a[1], this.config) < remToPx(b[1], this.config)) {
          return -1
        }

        return 0
      })
    }
  },
  methods: {
    getFontSizeValue (value) {
      // Tailwind 2.0 returns font size as array with size and line height
      if (Array.isArray(value)) {
        return {
          fontSize: value[0],
          ...value[1]
        }
      }

      return {
        fontSize: value
      }
    },
    getFontSizeString (value) {
      return Object.values(this.getFontSizeValue(value)).join(', ')
    }
  }
}
</script>
