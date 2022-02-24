<template>
  <div>
    <slot></slot>
  </div>
</template>
<script>
import Emitter from '../../mixins/emitter.js'
import { findComponentDownward } from '../../utils/assist.js'
export default {
  name: 'iCheckboxGroup',
  mixins: [Emitter],
  props: {
    value: {
      type: Array,
      default () {
        return []
      }
    }
  },
  data () {
    return {
      currentdValue: this.value,
      childrens: []
    }
  },
  methods: {
    updateModel (update) {
      this.childrens = findComponentDownward(this, 'iCheckbox')
      if (this.childrens) {
        const { value } = this
        this.childrens.forEach(child => {
          child.model = value
          if (update) {
            child.currentdValue = value.indexOf(child.label) >= 0
            child.group = true
          }
        })
      }
    },
    change (data) {
      this.currentdValue = data
      this.$emit('input', data)
      this.$emit('on-change', data)
      this.dispatch('iFormItem', 'on-form-change', data)
    }
  },
  mounted () {
    this.updateModel(true)
  },
  watch: {
    value () {
      this.updateModel(true)
    }
  }
}
</script>

<style>
</style>