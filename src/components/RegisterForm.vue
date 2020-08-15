<template>
  <div class="register">
    <form class="main_form" name="new">
      <span>Поля, отмеченные звёздочками, являются обязательными</span>
      <Input inp_type="text"  inp_name="last_name" @onUpdate="update" inp_placeholder="Фамилия" :min_length=2 :isRequired=true />
      <Input inp_type="text" inp_name="name" @onUpdate="update" inp_placeholder="Имя" :min_length=2 :isRequired=true />
      <Input inp_type="text"  inp_name="second_name" @onUpdate="update" inp_placeholder="Отчество" :isRequired=false />
      <Input inp_type="date"  inp_name="birth"  @onUpdate="update" inp_placeholder="Дата рождения" :isRequired=true /> <!--Дата рождения-->
      <Input inp_type="tel"  inp_name="tel" @onUpdate="update" inp_placeholder="Телефон" :isRequired=true />
      <Select inp_name="sex" inp_placeholder="Пол" @onUpdate="update" :options="[{name: 'Мужской', value: 'Male'}, {name: 'Женский', value: 'Famale'}]" :isRequired=false />
      <!--СДЕЛАТЬ КОМПОНЕНТ МУЛЬТИСЕЛЕКТОР<s inp_type="selector multy"  inp_name="group" inp_placeholder="Группа пациентов" :isRequired=false />-->
      <Input inp_type="email"  inp_name="email" @onUpdate="update" inp_placeholder="Email" :min_length=6 :isRequired=true />
      <Select inp_name="пол" :options="[{name: 'Мужской', value: 'Male'}, {name: 'Женский', value: 'Famale'}]" inp_placeholder="Пол" :isRequired=false />
      <!--КОМПОНЕНТ ЧЕКБОКС, допилить как нибудь в компонент инпут<Input inp_type="text"  inp_name="sms" inp_placeholder="не отправлять смс" :isRequired=false />-->
      <Select inp_name="doctor" @onUpdate="update" inp_placeholder="Лечащий врач" :options="[{name: 'Иванов', value: 'Ivanov'}, {name: 'Захаров', value: 'Zakhar'}, {name: 'Чернышов', value: 'Chernysh'}]" :isRequired=true />
    </form>
  </div>
</template>

<script>
import Input from "../components/Input.vue"
import Select from "../components/Selector.vue";

export default {
  name: "RegisterForm",
  data() {
   return  {
     
   }
  },
  methods: {
    update(data){
      console.log(this)
      console.log(data);
      this.$data[data.name] = {
       isValid: data.isValid,
        val: data.inp
      }
      console.log(this.$data)
    }
  },
  components: {
    Input,
    Select
  },
};
</script>


<style scoped lang="scss">
.main_form {
  margin: auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background: white;
  padding: 10px 40px;
  border-radius: 15px;
  width: 40%;
}

span {
  margin: 0 0 15px 0;
}
@media screen and(max-width: 530px) {
  .main_form {
    width: 70%;
  }
}
</style>
