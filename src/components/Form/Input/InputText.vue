<template>
  <label class="label">
    <span v-if="$slots.default">
      <slot />
    </span>

    <input
      :value="value"

      class="input"
      :class="classNameInput"
      v-on="listeners"
      v-bind="$attrs"
    >

    <span v-if="$slots.append">
      <slot name="append" />
    </span>
  </label>
</template>

<script>
export default {
  name: 'InputText',
  props: {
    value: {
      type: String,
      required: true,
    },
    classNameInput: {
      type: String,
      default: undefined,
    },
  },
  inheritAttrs: false,
  computed: {
    listeners() {
      return {
        ...this.$listeners,
        input: e => this.$emit('input', e.target.value),
      };
    },
  },
};
</script>

<style scoped>
.label {
  display: inline-flex;
  flex-direction: column;
  width: 300px;

  text-align: left;
}

.label + .input {
  margin-top: 8px;
}

.input {
  width: 100%;
  padding: 4px 12px;

  font-size: 16px;

  border-radius: 4px;
  border: 1px solid cornflowerblue;
  outline: none;
}

.input:focus,
.input:focus-visible {
  box-shadow: 0 0 4px cornflowerblue;
}
</style>
