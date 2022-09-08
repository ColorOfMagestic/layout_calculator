<template>
  <input
    class="calc_input calc_input_block calc_input--gen"
    type="number"
    min="0"
    @input="$emit('input', $event.target.value)"
    ref="input"
  />
</template>

<script>
export default {
  name: 'InputText',
  props: {
    model: {
      prop: 'value',
      event: 'input'
    },

    price: {
      type: Number
    }
  }
}
</script>

<style lang="sass" scoped></style>
