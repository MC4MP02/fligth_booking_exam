<script>
export default {
  props: ['modelValue', 'thresholds'],
  emits: ['update:modelValue'],
  data() {
    return {
      data: {},
      minMax: {
        min: this.modelValue.min,
        max: this.modelValue.max
      }
    }
  },
  created() {
    this.init()
  },
  watch: {
    modelValue: {
      handler: function (val) {
        this.minMax = val
        this.init()
      },
      deep: true
    },
    /*
      'modelValue.min'(min) {
        this.minMax.min = min
        this.init()
      },
      'modelValue.max'(max) {
        this.minMax.max = max
        this.init()
      }
    */
  },
  methods: {
    init() {
      this.thresholds.forEach((el) => {
        let objForm = ''
        let objColor = 'black'

        if (el == this.minMax.min || el == this.minMax.max) {
          objForm = '●'
        } else if (el > this.minMax.min && el < this.minMax.max) {
          objForm = '—'
        }
        this.data[el] = {
          form: objForm,
          color: objColor
        }
      })
    },
    handleClick(el) {
      if (this.data[el].form == '●') {
        this.data[el].color == 'black' ? this.data[el].color = 'green' : this.data[el].color = 'black'

        if (el == this.minMax.min) {
          if (this.data[this.minMax.max].color == 'green') {
            this.data[this.minMax.max].color = 'black'
          }
        }
        if (el == this.minMax.max) {
          if (this.data[this.minMax.min].color == 'green') {
            this.data[this.minMax.min].color = 'black'
          }
        }

      } else {
        let min = this.minMax.min
        let max = this.minMax.max

        if (this.data[this.minMax.min].color == 'green') {
          this.data[this.minMax.min].color == 'black'
          if (el > this.minMax.max) {
            this.minMax = {
              min: max,
              max: el
            }
          } else {
            this.minMax = {
              min: el,
              max: max
            }
          }
        } else if (this.data[this.minMax.max].color == 'green') {
          this.data[this.minMax.max].color == 'black'
          if (el < this.minMax.min) {
            this.minMax = {
              min: el,
              max: min
            }
          } else {
            this.minMax = {
              min: min,
              max: el
            }
          }
        }
        this.$emit('update:modelValue', this.minMax)
        this.init()
      }
    }
  }
}
</script>

<template>
  <div class="range">
    <div v-for="(el) in thresholds">
      <div class="range-item" @click="handleClick(el)" :style="`color: ${this.data[el].color}`">{{ data[el].form }}
      </div>
      <span class="range-threshold">{{ el }}</span>
    </div>
  </div>
</template>

<style></style>