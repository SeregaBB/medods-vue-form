<template>
  <div class="register">
    <form class="main_form" name="new">
      <span>Поля, отмеченные звёздочками, являются обязательными</span>
      <Input inp_type="text"  inp_name="last_name" inp_placeholder="Фамилия" :min_length=2 :isRequired=true />
      <Input inp_type="text" inp_name="name" inp_placeholder="Имя" :min_length=2 :isRequired=true />
      <Input inp_type="text"  inp_name="second_name" inp_placeholder="Отчество" :isRequired=false />
      <Input inp_type="date"  inp_name="birth" inp_placeholder="Дата рождения" :isRequired=true /> <!--Дата рождения-->
      <Input inp_type="tel"  inp_name="tel" inp_placeholder="Телефон" :isRequired=true />
      <Input inp_type="email"  inp_name="email" inp_placeholder="Email" :min_length=6 :isRequired=true />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
      <Input inp_type="text"  inp_name="city" inp_placeholder="Город" :isRequired=false />
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
import { helpers, required, minLength } from "vuelidate/lib/validators";
const alpha = helpers.regex("alpha", /^[а-яА-Яa-zA-Z]*$/);
const telValodator = helpers.regex("telValidator", /^7\d{10}/gi);

const defaultData = () => ({
  lastName: "",
  name: "",
  secondName: "",
  birthday: "",
  tel: "",
  sex: "Пол",
  group: ["Группа пациентов (обязательно)"],
  doctor: "Лечащий врач",
  sms: false,
  postCode: "",
  country: "",
  region: "",
  city: "",
  street: "",
  building: "",
  doctype: "Тип документа (обязательно)",
  serialNum: "",
  docNum: "",
  docWho: "",
  docdate: "",
  success: ""
});
export default {
  name: "RegisterForm",
  data: () => defaultData(),
  validations: {
    lastName: { alpha, required, minLength: minLength(2) },
    name: { alpha, required, minLength: minLength(2) },
    birthday: { required },
    group: { required },
    sms: false,
    city: { required, minLength: minLength(3), alpha },
    doctype: { required },
    docdate: { required },
    tel: { required, telValodator, minLength: minLength(11) }
  },

  ///методы
  methods: {
    resetData() {
      Object.assign(this.$data, defaultData());
    },

    //при сабмите формы забираем все введённые данные в форму + выводим сообщение "новый клиент успешно создан"
    onSubmit() {
      const formData = {};
      this.$v.$touch();
      if (!this.$v.$anyError) {
        for (let key in this.$v) {
          if (!key.includes("$")) {
            formData[key] = this.$v[key].$model;
          }
        }

        this.$v.$reset();
        this.resetData();

        this.success = "Новый клиент успешно создан";
        setTimeout(() => {
          this.success = "";
        }, 3000);
      }
    }
  },
  //конец методов
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
  select {
    > option {
      &:disabled {
        background: none;
      }
    }
    box-sizing: border-box;
    margin: 0 0 10px;
    width: 100%;
    padding: 10px;
    border: none;
    outline: none;
    border-radius: 10px;
    font-weight: bolder;
    box-shadow: 0 4px 5px #ccc;
    -webkit-box-shadow: 0 4px 5px #ccc;
    -webkit-appearance: none;
  }
  input {
    box-sizing: border-box;
    margin: 0 0 10px;
    width: 100%;
    padding: 10px;
    border: none;
    outline: none;
    border-radius: 10px;
    font-weight: bolder;
    box-shadow: 0 4px 5px #ccc;
    -webkit-box-shadow: 0 4px 5px #ccc;
    -webkit-appearance: none;
    &::placeholder {
      font-weight: bolder;
      color: #000;
    }
  }
  input.invalid {
    box-shadow: 0 4px 5px rgb(255, 129, 129);
    -webkit-box-shadow: 0 4px 5px rgb(255, 129, 129);
    -webkit-appearance: none;
  }
  select.invalid {
    box-shadow: 0 4px 5px rgb(255, 129, 129);
    -webkit-box-shadow: 0 4px 5px rgb(255, 129, 129);
    -webkit-appearance: none;
  }
  input[type="checkbox"] {
    align-self: flex-start;
    -webkit-appearance: checkbox;
  }
  input[type="date"] {
    width: 100%;
  }
  span {
    width: 100%;
    font-size: 15px;
    font-weight: bolder;
    text-shadow: 0 3px 3px white;
    margin: 0 0 10px 0;
  }
  small {
    width: 100%;
    color: rgb(255, 129, 129);
  }
  button {
    width: 100%;
    margin: 0 0 10px;
    padding: 10px;
    border: none;
    outline: none;
    border-radius: 10px;
    font-weight: bolder;
    box-shadow: 0 4px 5px #ccc;
    -webkit-box-shadow: 0 4px 5px #ccc;
    -webkit-appearance: none;
    background: yellowgreen;
    cursor: pointer;
    &:hover {
      transition: 0.4s;
      background: rgb(102, 165, 2);
    }
    &:active {
      background: rgb(67, 105, 1);
    }
    &:disabled {
      cursor: not-allowed;
    }
  }
  .group {
    width: 100%;
    align-self: start;
    text-align: left;
    label {
      width: 100%;
      margin: 0 0 0 5px;
    }
    input {
      width: unset;
      border-radius: 0;
    }
  }
  label {
    width: 100%;
    text-align: left;
  }
  input.required {
    box-shadow: rgb(73, 151, 253);
  }
}
@media screen and(max-width: 530px) {
  .main_form {
    width: 70%;
  }
}
</style>
