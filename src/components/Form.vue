<script lang="ts">
  import { defineComponent } from "vue";
  import { JsonForms, JsonFormsChangeEvent } from "@jsonforms/vue";
  import { defaultStyles, mergeStyles, vanillaRenderers, } from "@jsonforms/vue-vanilla";
  import { rankWith, isStringControl, scopeEndsWith } from "@jsonforms/core"

  // Import Custom Renderer that uses Kendo UI
  import stringControlRenderer from "../renderers/String.vue"
  import dateControlRenderer from "../renderers/Date.vue"

  import { Button as KButton } from '@progress/kendo-vue-buttons'

  // Import JSON Schemas
  import schema from '../Schema/Schema.json'
  import uischema from '../Schema/UISchema.json'

  // mergeStyles combines all classes from both styles definitions into one
  const myStyles = mergeStyles(defaultStyles, {
    control: {
      label: "jrcgonzalez-suntech"
    }
  });

  const renderers = [
    ...vanillaRenderers,
    // Use String Custom Renderer
    {
      tester: rankWith(
        2,
        isStringControl,
      ),
      renderer: stringControlRenderer,
    },
    // Use Datepicker Custom Renderer
    {
      tester: rankWith(
        3,
        scopeEndsWith('birthDay'),
      ),
      renderer: dateControlRenderer,
    },
    // here you can add custom renderers
  ];

  export default defineComponent({
    name: "App",
    components: {
      JsonForms,
      KButton,
    },
    data() {
      return {
        // freeze renderers for performance gains
        renderers: Object.freeze(renderers),
        data: {
          firstName: undefined,
          lastName: undefined,
          birthDay: undefined,
          email: undefined,
        },
        schema,
        uischema,
      };
    },
    mounted() {
      // Initially Hide email field
      const emailField = document.getElementById('email');
      if (emailField) emailField.style.display = 'none';
    },
    methods: {

      // Handle jsonforms changes/event
      onChange(event: JsonFormsChangeEvent) {
        // Temporarily store current birthday as previous
        let prevBirthDay = this.data.birthDay;
        // Update data from jsonforms event
        this.data = event.data;
        // Check if birthDay field is changed, if yes, calculate age and show/hide email field if age is 18+.
        if (event.data.birthDay != prevBirthDay) {
          if (this.computeAge() < 18) {
            const emailField = document.getElementById('email');
            if (emailField) emailField.style.display = 'none';
          } else {
            const emailField = document.getElementById('email');
            if (emailField) emailField.style.display = 'block';
          }
        }
      },

      // Compute if age using today and selected birthDay
      computeAge() {
        if (this.data.birthDay === undefined) return 0;
        let parsedBirthday = new Date(this.data.birthDay);
        let age = Date.now() - parsedBirthday.getTime();
        let parseAge = new Date(age);
        return Math.abs(parseAge.getUTCFullYear() - 1970);
      },

      // Show message if next button is clicked
      showMessage() {
        alert('Your age is 18 or above!');
      }
    },

    provide() {
      return {
        styles: myStyles,
      };
    },
  });
</script>

<template>
  <div id="regForm">
    <h1>Registration Form</h1>
    <div class="jsonForms-jrcg">
      <json-forms :data="data" :renderers="renderers" :schema="schema" :uischema="uischema" @change="onChange" />
    </div>
    <KButton :theme-color="'primary'" :disabled="(computeAge() < 18 ? true : false)">Next</KButton>
  </div>
</template>

<style scoped>
  #regForm {
    background-color: #ffffff;
    padding: 2rem;
    border: 1px solid #f0f0f0;
  }
</style>
