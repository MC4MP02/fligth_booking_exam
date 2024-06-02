<script>
/*
Note: use this syntax to watch a nested field

   watch: {
    'object.field': function() { ...  },
*/
export default {
  props: ['modelValue', 'thresholds'],
  emits: ['update:modelValue'],
  data() {
    return {
      forms: {},
      formColor: 'black'
    }
  },
  created() {
    this.initForms()
  },
  methods: {
    handleClick(value) {
      let min = this.modelValue.min
      let max = this.modelValue.max

      if (this.forms[value].form == '●') {
        this.forms[value].color == 'black' ? this.forms[value].color = 'green' : this.forms[value].color = 'black'
      } else {
        if (this.forms[min].color == 'green' && this.forms[value] != '●') {
          if (value > max) {
            this.$emit('update:modelValue', { min: max, max: value })
          } else {

            this.$emit('update:modelValue', { min: value, max: max })
          }
        } else if (this.forms[max].color == 'green' && this.forms[value] != '●') {
          if (value < min) {
            this.$emit('update:modelValue', { min: value, max: min })
          } else {
            this.$emit('update:modelValue', { min: min, max: value })
          }
        }
      }

      if (value == min) {
        this.forms[max].color = 'black'
      } else if (value == max) {
        this.forms[min].color = 'black'
      }
    },
    initForms() {
      this.thresholds.forEach((el) => {
        if (el == this.modelValue.min || el == this.modelValue.max) {
          this.forms[el] = {
            form: '●',
            color: 'black'
          }
        } else if (el > this.modelValue.min && el < this.modelValue.max) {
          this.forms[el] = {
            form: '—',
            color: 'black'
          }
        } else {
          this.forms[el] = {
            form: '',
            color: 'black'
          }
        }
      })
    }
  },
  watch: {
    modelValue() {
      this.initForms()
    }
  }

}
</script>

<template>
  <div class="range">
    <div v-for="value, index in thresholds">
      <div class="range-item" @click="handleClick(value)" :style="`color: ${forms[value].color}`">
        {{ forms[value].form }}
      </div>
      <span class="range-threshold">
        {{ value }}
      </span>
    </div>
  </div>
</template>

<style></style>