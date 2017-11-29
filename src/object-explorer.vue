<template>
  <div class="object-explorer">
    <div
      class="object-explorer__property"
      v-for="objProperty in objProperties"
      :key="objProperty.name"
    >
      <div class="object-explorer__property-name">
        <input
          :value="objProperty.name"
          @blur="updatePropertyName(objProperty.name, $event.target.value)"
        />
      </div>
      <div class="object-explorer__property-value" v-if="!objProperty.isObject">
        <input
          :value="obj[objProperty.name]"
          @input="updatePropertyValue(objProperty.name, $event.target.value)"
        />
      </div>
      <object-explorer
        class="object-explorer__sub-object"
        v-if="objProperty.isObject"
        :obj="obj[objProperty.name]"
        @objUpdated="val => updatePropertyValue(objProperty.name, val)"
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
  },
  methods: {
    updatePropertyValue (propertyName, newValue) {
      this.$emit(
        'objUpdated',
        Object.assign({}, this.obj, {[propertyName]: newValue})
      );
    },
    updatePropertyName (oldName, newName) {
      var newObject = Object.assign({}, this.obj, {[newName]: this.obj[oldName]});
      delete newObject[oldName];
      this.$emit(
        'objUpdated',
        newObject
      )
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
  input {
    font-weight: bold;
    width: 100px;
  }

  &::after {
    content: ':';
    margin-right: 1em;
  }
}
</style>
