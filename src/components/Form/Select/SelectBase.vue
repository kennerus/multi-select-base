<template>
  <div class="select">
    <div>
      <ButtonBase
        class="btn_toggle-select"

        @click="toggleSelect"
      >
        {{ valueString }}
      </ButtonBase>
    </div>

    <div v-if="isOpen">
      <InputText
        v-if="search"
        v-model="searchString"
        placeholder="Поиск"
      />

      <ul class="list-reset">
        <li
          v-for="listElement in filteredList"
          :key="listElement.value"
        >
          <ButtonBase
            class="btn_list-item"
            :class="{'btn_list-item-active': isElementSelected(listElement.value)}"

            @click="handleSelection(listElement.value)"
          >
            {{ listElement.label }}
          </ButtonBase>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import cloneDeep from 'lodash/cloneDeep';
import ButtonBase from '../Button/ButtonBase';
import InputText from '../Input/InputText';

export default {
  name: 'SelectBase',
  components: { InputText, ButtonBase },
  props: {
    value: {
      type: [Number, String, Array],
      required: false,
    },
    list: {
      type: Array,
      /**
       * object[]
       * {
       *   label: string,
       *   value: number | string
       * }
       */
      required: true,
    },
    search: {
      type: Boolean,
      default: false,
    },
    multiple: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      searchString: '',
      isOpen: false,
    };
  },
  computed: {
    isValueArray() {
      return Array.isArray(this.value);
    },

    filteredList() {
      if (this.searchString === '') {
        return this.list;
      }

      return this.list.filter(el => el.label.toLowerCase().includes(this.searchString.toLowerCase()));
    },

    /**
     * Приведение выбранного значения к строке
     *
     * @return {string}
     */
    valueString() {
      if (this.value?.length === 0 || !this.value) {
        return 'Выберите значение';
      }

      if (this.isValueArray) {
        return this.value.map(value => this.findValueInItems(value)?.label).join('; ');
      } else {
        return this.findValueInItems(this.value)?.label;
      }
    },
  },
  methods: {
    toggleSelect() {
      this.isOpen = !this.isOpen;
    },

    /**
     * @param {string | number} value
     * @return {string | number | (string | number)[]}
     */
    getValueToEmit(value) {
      if (this.multiple) {
        const newValue = cloneDeep(this.value);
        const valueIndex = newValue.findIndex(this.isValuesEqual(value));
        valueIndex === -1 ? newValue.push(value) : newValue.splice(valueIndex, 1);

        return newValue;
      }

      return this.value === value ? '' : value;
    },

    /**
     * @param {string | number} value
     */
    emitSelectedValue(value) {
      this.$emit('input', this.getValueToEmit(value));
    },

    /**
     * @param {string | number} value
     */
    handleSelection(value) {
      if (this.multiple && !this.isValueArray) {
        throw new Error('value should be an Array');
      }

      this.emitSelectedValue(value);

      if (this.multiple === false) {
        this.toggleSelect();
      }
    },

    /**
     * @param {string | number} elementValue
     * @return {boolean}
     */
    isElementSelected(elementValue) {
      if (this.multiple && !this.isValueArray) {
        throw new Error('value should be an Array');
      }

      const { value, multiple } = this;

      return multiple ? Boolean(value.find(this.isValuesEqual(elementValue))) : value === elementValue;
    },

    /**
     *
     * @param {string} value
     * @return {function({label: string, value: string}): boolean}
     */
    isValuesEqual(value) {
      return el => el === value;
    },

    /**
     * @param {string} value
     * @return {object}
     */
    findValueInItems(value) {
      return this.list.find(el => el.value === value);
    },
  },
};
</script>

<style scoped>
.select {
  width: 300px;
}
</style>
