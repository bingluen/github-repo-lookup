<template>
  <div id="search-box">
    <b-form @submit.prevent="() => true">
      <b-form-group>
        <b-form-input
          v-model="keyword"/>
      </b-form-group>
    </b-form>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  name: 'SearchBox',
  props: {
    keywordChanged: Function,
  },
  data() {
    return {
      keyword: '',
    };
  },
  watch: {
    keyword(newValue) {
      this.submitKeyword(this, newValue);
    },
  },
  methods: {
    submitKeyword: _.debounce((that, newValue) => {
      that.$emit('keywordChanged', newValue);
    }, 500),
  },
};
</script>
