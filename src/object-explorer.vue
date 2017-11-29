<template>
  <div class="object-explorer">
    <div
      class="object-explorer__property"
      v-for="objProperty in objProperties"
      :key="objProperty.name"
    >
      <div class="object-explorer__property-name"> {{ objProperty.name }} </div>
      <div class="object-explorer__property-value" v-if="!objProperty.isObject">
        {{ obj[objProperty.name] }}
      </div>
      <object-explorer
        class="object-explorer__sub-object"
        v-if="objProperty.isObject"
        :obj="obj[objProperty.name]"
      ></object-explorer>
    </div>
  </div>
</template>

<script>
export default {
  name: 'object-explorer',
  props: {
    obj: {
      type: Object,
      default () { return {} }
    }
  },
  computed: {
    objProperties () {
      var propertyNames = Object.keys(this.obj);
      return propertyNames.map((k) => {
        return {
          name: k,
          isObject: this.obj[k] instanceof Object
        };
      });
    }
  }
}
</script>

<style lang="scss">
.object-explorer {
  text-align: left;
  margin-top: 1em;
}

.object-explorer__property {
  display: flex;
  flex-direction: row;
}

.object-explorer__property-name {
  font-weight: bold;

  &::after {
    content: ':';
    margin-right: 1em;
  }
}
</style>
