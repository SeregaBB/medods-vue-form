<template>
  <div class="register">
    <form class="main_form" name="new">
      <span>Поля, отмеченные звёздочками, являются обязательными</span>
      <Input inp_type="text"  inp_name="last_name" inp_placeholder="Фамилия" :min_length=2 :isRequired=true />
      <Input inp_type="text" inp_name="name" inp_placeholder="Имя" :min_length=2 :isRequired=true />
      <Input inp_type="text"  inp_name="second_name" inp_placeholder="Отчество" :isRequired=false />
      <Input inp_type="date"  inp_name="birth" inp_placeholder="Дата рождения" :isRequired=true /> <!--Дата рождения-->
      <Input inp_type="tel"  inp_name="tel" inp_placeholder="Телефон" :isRequired=true />
      <!-- СДЕЛАТЬ КОМПОНЕНТ СЕЛЕКТОР <Input inp_type="select"  inp_name="sex" inp_placeholder="Пол" :isRequired=false />--> 
      <!--СДЕЛАТЬ КОМПОНЕНТ МУЛЬТИСЕЛЕКТОР<s inp_type="selector multy"  inp_name="group" inp_placeholder="Группа пациентов" :isRequired=false />-->
      <Input inp_type="email"  inp_name="email" inp_placeholder="Email" :min_length=6 :isRequired=true />
      <!-- СДЕЛАТЬ КОМПОНЕНТ СЕЛЕКТОР <Input inp_type="select"  inp_name="doctor" inp_placeholder="Лечащий врач" :isRequired=false />--> 
      <!--КОМПОНЕНТ ЧЕКБОКС, допилить как нибудь в компонент инпут<Input inp_type="text"  inp_name="sms" inp_placeholder="не отправлять смс" :isRequired=false />-->
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
    </form>
  </div>
</template>

<script>
import Input from "../components/Input.vue"

export default {
  name: "RegisterForm",
  components: {
    Input
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
