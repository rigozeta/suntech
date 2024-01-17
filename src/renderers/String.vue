<template>
    <div class="form-field" :id="control.path">
        <label>{{ control.label }}</label>
        <KInput
        :id="control.id"
        :hint="control.description"
        :disabled="!control.enabled"
        :error-messages="control.errors"
        :value="control.data"
        :onInput="onChange"
        ></KInput>
        <span class="desc" :class="(control.errors ? 'hasErrors':'')">{{ (!control.errors ? control.description : control.errors) }}</span>
    </div>
  </template>

<script lang="ts">
  import {
    ControlElement,
    JsonFormsRendererRegistryEntry,
    rankWith,
    isStringControl,
  } from '@jsonforms/core';
  import { defineComponent } from 'vue';
  import {
    rendererProps,
    useJsonFormsControl,
    RendererProps,
  } from '@jsonforms/vue';

  import { Input } from '@progress/kendo-vue-inputs'
  
  const controlRenderer = defineComponent({
    name: 'string-control-renderer',
    components: {
        KInput: Input,
    },
    props: {
      ...rendererProps<ControlElement>(),
    },
    setup(props: RendererProps<ControlElement>) {
      return useJsonFormsControl(props)
    },
    methods: {
        onChange(event: Event) {
            this.handleChange(this.control.path, (event.target as HTMLInputElement).value)
        }
    },
  });
  
  export default controlRenderer;
  
  export const entry: JsonFormsRendererRegistryEntry = {
    renderer: controlRenderer,
    tester: rankWith(1, isStringControl),
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