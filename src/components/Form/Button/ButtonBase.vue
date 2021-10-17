<template>
  <button
    class="btn"
    type="button"
    v-on="$listeners"
  >
    <slot />
  </button>
</template>

<script>
export default {
  name: 'ButtonBase',
};
</script>

<style scoped>
.btn {
  padding: 4px 12px;

  font-size: 16px;
  text-align: left;

  background-color: cornflowerblue;
  border: none;
  cursor: pointer;
}

.btn_toggle-select {
  width: 100%;

  background-color: transparent;
  border: 1px solid lightskyblue;
}

.btn_list-item {
  width: 100%;

  background-color: rgba(204, 204, 204, 0.25);
}

.btn_list-item:hover,
.btn_list-item:focus,
.btn_list-item:active {
  background-color: rgba(204, 204, 204, 0.5);
}

.btn_list-item-active {
  background-color: #ccc;
}
</style>
