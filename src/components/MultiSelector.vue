<template>
  <div :class="[{required: isRequired},'input_group']">
    <label :for="inp_name">{{inp_placeholder}}</label>
    <input 
    type="text"
    v-model="input_values"
    @click="toggleOptions"
    :class="[{input_error : $v.multi_selector.$error}]"
    >
    <div class="options">
        <div
    class="opt_group"
    v-for="(option,index) in options" 
    :key="index" 
    :value="option.value"
    :name="option.name"
        >
        <input type="checkbox" :name="option.name" :value="option.value" @change="onChangeCheckbox">
            <span>{{option.name}}</span>
        </div>
        
    </div>
   <!-- <select 
    multiple
    :name="inp_name"
    :class="[{input_error : $v.multi_selector.$error}]"
    v-model="multi_selector"
    @click="openOptions" 
    @change="validate"
    @blur="onBlur"
    >
    
    
    <option 
    
    v-for="(option,index) in options" 
    :key="index" 
    :value="option.value">
    {{option.name}}
    </option>

    </select>
-->
    <span class="error">
      {{errText}}
    </span>
  </div>
</template>



<script>
import { requiredIf } from "vuelidate/lib/validators";

export default {
  data() {
    return {
     errText: '',
     multi_selector: [],
     input_values: []
    };
  },
  mounted(){
      this.$emit('onUpdate', {
          inp: this.multi_selector,
          name: this.inp_name, 
          isValid: !this.$v.multi_selector.$error,
          isRequired: this.isRequired
    })
  },
  props: {
    inp_name: String, 
    inp_placeholder: String,
    isRequired: Boolean, 
    options: Array
  },

  methods: {
      onChangeCheckbox(event){
          this.validate()
          if(event.target.checked){
              this.multi_selector.push(event.target.value);
              this.input_values.push(event.target.name);
              return;
          }
          if(!event.target.checked){
              this.multi_selector = this.multi_selector.filter(item=> item !== event.target.value);
              this.input_values = this.input_values.filter(item=> item !== event.target.name);
              return;
          }

      },

     
      toggleOptions(event){
         event.target.blur();
         this.validate();
          this.$el.querySelector('.options').classList.toggle('opened');

      },
      validate(){
          this.$v.$touch();
          if (!this.$v.multi_selector.required) this.errText = `Выберите хотя бы один из вариантов в поле "${this.inp_placeholder}"`;
          if (!this.$v.multi_selector.$error) this.errText = '';


          this.$emit('onUpdate', {
          inp: this.multi_selector,
          name: this.inp_name, 
          isValid: !this.$v.multi_selector.$error
          })
      },
  },
  validations() {
     return {
      multi_selector: {
        required: requiredIf(multi_selector => {
          return multi_selector.isRequired;
        })
  }
}
  }
}
</script>


<style lang="scss">
$borderColor: #ccc;
$errorColor: rgb(252, 119, 119);
   input{
       word-spacing: 10px;
   }
   input[type="checkbox"] {
        background: unset;
   width: 20px;
   border-radius: 50px;
   outline: none;
   border: none;
   box-shadow: none;
   -webkit-appearance: checkbox;
    }
    .options {
    display: none;
    flex-direction: column;
    align-items: flex-start;
    position: absolute;
    top: 38px;
    background: #fff;
    z-index: 2;
    width: 95%;
    padding: 15px;
    box-shadow: 0 0 3px #ccc;
    border-radius: 15px;
      &.opened {
          display: flex;
      }
    }
    
</style>