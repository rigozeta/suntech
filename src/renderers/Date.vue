<template>
  <div class="form-field" :id="control.path">
    <label>{{ control.label }}</label>
    <datepicker :default-value="new Date()"
      :id="control.id + '-input'"
      :error-messages="control.errors"
      :value="control.data"
      @change="onChange"
    ></datepicker>
    <span class="desc">{{ control.description }}</span>
  </div>
</template>

<script lang="ts">
import {
  ControlElement,
  JsonFormsRendererRegistryEntry,
  rankWith,
  scopeEndsWith,
} from '@jsonforms/core';
import { defineComponent } from 'vue';
import {
  rendererProps,
  useJsonFormsControl,
  RendererProps,
} from '@jsonforms/vue';
import { DatePicker } from '@progress/kendo-vue-dateinputs'

const controlRenderer = defineComponent({
  name: 'date-control-renderer',
  components: {
    'datepicker': DatePicker,
  },
  props: {
    ...rendererProps<ControlElement>(),
  },
  setup(props: RendererProps<ControlElement>) {
    return useJsonFormsControl(props);
  },
  methods: {
      onChange(event: Event) {
        console.log("WWW--", event)
          this.handleChange(this.control.path, (event.target as HTMLInputElement).value)
      }
  },
});

export default controlRenderer;

export const entry: JsonFormsRendererRegistryEntry = {
  renderer: controlRenderer,
  tester: rankWith(1, scopeEndsWith('birthDay')),
};
</script>

<style>
.form-field {
  margin-bottom: 1rem;
}
.desc {
  font-size: 0.8em;
  opacity: 0.8;
}
.hasErrors {
  color: red;
}
</style>