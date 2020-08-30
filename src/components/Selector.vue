<template>
  <div :class="[{ required: isRequired }, 'input_group']">
    <label :for="inp_name">{{ inp_placeholder }}</label>

    <select
      :name="inp_name"
      :class="[{ input_error: $v.selector.$error }]"
      v-model="selector"
      @click="validate"
      @change="validate"
    >
      <option
        v-for="(option, index) in options"
        :key="index"
        :value="option.value"
      >
        {{ option.name }}
      </option>
    </select>
    <span class="error">{{ errText }}</span>
  </div>
</template>

<script>
import { requiredIf } from "vuelidate/lib/validators";

export default {
  data() {
    return {
      errText: "",
      selector: ""
    };
  },
  mounted() {
    this.$emit("onUpdate", {
      inp: this.selector,
      name: this.inp_name,
      isValid: !this.$v.selector.$error,
      isRequired: this.isRequired
    });
  },
  props: {
    inp_name: String,
    inp_placeholder: String,
    isRequired: Boolean,
    isMultiple: Boolean,
    options: Array
  },

  methods: {
    refreshInput() {
      this.selector = "";
      this.errText = "";
      if (this.isRequired) this.$v.$reset();
    },
    validate() {
      this.$v.$touch();
      if (!this.$v.selector.required)
        this.errText = `Выберите один из вариантов в поле "${this.inp_placeholder}"`;
      if (!this.$v.selector.$error) this.errText = "";

      this.$emit("onUpdate", {
        inp: this.selector,
        name: this.inp_name,
        isValid: !this.$v.selector.$error
      });
    }
  },
  validations() {
    return {
      selector: {
        required: requiredIf(selector => {
          return selector.isRequired;
        })
      }
    };
  }
};
</script>

<style lang="scss">
$borderColor: #ccc;
$errorColor: rgb(252, 119, 119);
select {
  border-radius: 15px;
  outline: none;
  border: none;
  box-shadow: 0 0 3px $borderColor;
  z-index: 0;
  padding: 10px;
  -webkit-box-shadow: 0 0px 3px $borderColor;
  -webkit-appearance: none;
  &.input_error {
    box-shadow: 0 0 3px $errorColor;
  }
}
</style>
