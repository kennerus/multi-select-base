<template>
  <div class="home">
    <SelectBase
      v-model="value"
      :list="list"
      search
    />
  </div>
</template>

<script>
import InputText from '../components/Form/Input/InputText';
import SelectBase from '../components/Form/Select/SelectBase';

export default {
  name: 'Home',
  components: {
    SelectBase,
    InputText,
  },
  data() {
    return {
      value: [],
      list: [
        {
          label: 'Один',
          value: 'one',
        },
        {
          label: 'Два',
          value: 'two',
        },
        {
          label: 'Три',
          value: 'three',
        },
      ],
    };
  },
};
</script>
