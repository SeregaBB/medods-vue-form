<template>
  <div :class="[{required: isRequired},'input_group']">
    <label :for="inp_name">{{inp_placeholder}}</label>
    <input 
    :type="inp_type" 
    :name="inp_name" 
    
    @input="validate" 
    v-model="inp" 
    :class="[{input_error : $v.inp.$error}]" 
    />

    <span class="error">
      {{errText}}
    </span>
  </div>
</template>



<script>
import {  requiredIf, minLength, maxLength } from "vuelidate/lib/validators";
export default {
  data() {
    return {
      inp: "",
      errText: ""
    };
  },
  props: {
    inp_type: String,
    inp_name: String,
    min_length: Number,
    max_length: Number,
    isRequired: Boolean,
    inp_placeholder: String,
    special_pattern: String,
    special_format: String
  },

  methods: {
    validate() {
      this.$v.$touch();
      console.log(this.$v.inp);

      if (!this.$v.inp.required) this.errText = `Заполните поле "${this.inp_placeholder}"`
      if (!this.$v.inp.minLength) this.errText = `${this.inp_placeholder.toLowerCase()} не может быть короче ${this.min_length} символов. Сейчас введено ${this.inp.length}`
      if (!this.$v.inp.maxLength) this.errText = `${this.inp_placeholder.toLowerCase()} не может быть длиннее ${this.max_length} символов. Сейчас введено ${this.inp.length}`
      if (!this.$v.inp.spec_validator) this.errText = `Заполните поле в формате ${this.special_format}`

      if (!this.$v.inp.$error) this.errText = "";

        this.$emit('onUpdate', {
          inp: this.inp,
          name: this.inp_name, 
          isValid: !this.$v.inp.$error
        })
    }
  },
  validations() {
    return {
      inp: {
        required: requiredIf(input => {
          return input.isRequired;
        }),
        minLength: minLength(
          this.min_length && this.isRequired ? this.min_length : 0
        ),
        maxLength: maxLength(
          this.max_length ? this.max_length: 999
        ),
        spec_validator:  (input) => {
             if (this.special_pattern) {
               const reg = new RegExp(this.special_pattern, 'gi');
               return reg.test(input);
             }
             return true;
        }
      }
    };
  }
};
</script>


<style lang="scss">
$borderColor: #ccc;
$errorColor: rgb(252, 119, 119);

.input_group {
  display: flex;
  flex-direction: column;
  position: relative;
  &.required {
     &::before {
        content: ('*');
        color: $errorColor;
        position: absolute;
        top: 15%;
        right: 1%;
        z-index: 1;
     }
  }
    label {
      position: absolute;
      z-index: 1;
      top: -20%;
      left: 2%;
      background: #fff;
    }
    .error {
      display: block;
      height: 30px;
      color: $errorColor;
    }
    input {
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
    
}

</style>