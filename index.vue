<template>
  <div>
    <h3>{{ title }}</h3>

    <!-- mode list -->
    <ul v-if="isModeList">
      <li v-for="val in value">{{ val }}</li>
    </ul>

    <!-- mode map -->
    <dl v-else-if="isModeMap">
      <template v-for="key in mapsKeys">
        <dt :title="key">{{ key }}</dt>
        <dd :title="value[key]">{{ value[key] }}</dd>
      </template>
    </dl>

    <!-- mode simple -->
    <p v-else>{{ value }}</p>

  </div>
</template>

<script>

const MODE = {
  SIMPLE: 0,
  LIST: 1,
  MAP: 2
};

export default {

  // Props
  props: {
    title: {
      type: String,
      required: true
    },

    data: {
      type: Array,
      required: true
    },

    params: {
      default: null
    }
  },

  // Computed
  computed: {

    /**
     * Value
     * @returns {*}
     */
    value () {
      return this.data.length ? this.data[this.data.length - 1].state : null;
    },

    /**
     * Mode
     * @returns {number}
     */
    mode () {
      let mode = MODE.SIMPLE;

      if (this.value && typeof this.value === 'object') {
        mode = Array.isArray(this.value) ? MODE.LIST : MODE.MAP;
      }

      return mode;
    },

    /**
     * Returns is mode simple?
     * @returns {boolean}
     */
    isModeSimple () {
      return this.mode === MODE.SIMPLE;
    },

    /**
     * Returns is mode list?
     * @returns {boolean}
     */
    isModeList () {
      return this.mode === MODE.LIST;
    },

    /**
     * Returns is mode map?
     * @returns {boolean}
     */
    isModeMap () {
      return this.mode === MODE.MAP;
    },

    /**
     * Map's keys
     * @returns {string[]}
     */
    mapsKeys () {
      return Object.keys(this.value).sort();
    }
  }
}
</script>

<style lang="less" scoped>
@import '~@less/helpers.less';

dl {
  margin: 1em;
  
  dd {
    margin: 0 0 0.5em;
    font-size: 3em;
    .ellipsis;
  }
}
</style>
