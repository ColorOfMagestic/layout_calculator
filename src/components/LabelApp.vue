<template>
  <label class="calc_label">{{ title }}<slot></slot></label>
</template>

<script>
export default {
  name: 'LabelApp',
  props: {
    title: {
      type: String
    }
  }
}
</script>

<style lang="sass" scoped></style>
