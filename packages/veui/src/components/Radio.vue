<template>
<label
  :class="{
    'veui-radio': true,
    'veui-disabled': realReadonly || realDisabled
  }"
  :ui="ui">
  <input type="radio" v-bind="attrs" @change="localChecked = $event.target.checked">
  <span class="veui-radio-box"></span>
  <span class="veui-radio-label"><slot/></span>
</label>
</template>

<script>
import ui from '../mixins/ui'
import input from '../mixins/input'

export default {
  name: 'veui-radio',
  inheritAttrs: false,
  mixins: [ui, input],
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    value: {
      type: null,
      default: true
    },
    checked: null
  },
  data () {
    return {
      localChecked: this.checked
    }
  },
  computed: {
    attrs () {
      return {
        checked: this.localChecked,
        name: this.realName,
        disabled: this.realDisabled || this.realReadonly,
        ...this.$attrs
      }
    }
  },
  watch: {
    checked (value) {
      this.localChecked = value
    },
    localChecked (value) {
      if (this.checked !== value) {
        this.$emit('change', value)
      }
    }
  }
}
</script>
