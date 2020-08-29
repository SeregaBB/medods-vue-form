<template>
  <div :class="[{required: isRequired},'input_group']">
    <label :for="inp_name">{{inp_placeholder}}</label>
    <input 
    type="checkbox" 
    :name="inp_name" 
    v-model="inp" 
    @change="onChangeCheckbox"
    class="checkbox"
    />

    <span class="checkbox_text">
      {{checkbox_text}}
    </span>
  </div>
</template>



<script>
export default {
  data() {
    return {
      inp: false,
      checkbox_text: this.onFalse
    };
  },
  props: {
    inp_name: String,
    isRequired: Boolean,
    inp_placeholder: String,
    onTrue: String,
    onFalse: String
  },

  methods: {
    onChangeCheckbox(){
        if (this.inp) this.checkbox_text = this.onTrue //текст, который будет показан, когда чекбоск выбран
        else this.checkbox_text = this.onFalse // текст в противном случае
    }
  }
}
</script>


<style lang="scss" scoped>
input.checkbox {
   background: unset;
   width: 20px;
   border-radius: 50px;
   outline: none;
   border: none;
   box-shadow: none;
   -webkit-appearance: checkbox;
}
.input_group {
    display: flex;
    flex-direction: row;
    margin: 0 0 30px 0;
    box-shadow: 0 0 3px #ccc;
    border-radius: 19px;
    padding: 10px;
}
label {
    position: absolute;
    z-index: 1;
    top: -25%;
    left: 2%;
    background: #fff;
}
</style>