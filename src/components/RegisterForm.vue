<template>
  <div class="register">
    <form class="main_form" name="new" @submit="onSubmit">
      <span class="warn">
        Поля, отмеченные звёздочками
        <span class="star">*</span>, являются обязательными
      </span>
      <p>Основная информация</p>
      <Input
        inp_type="text"
        inp_name="last_name"
        @onUpdate="update"
        inp_placeholder="Фамилия"
        :min_length="2"
        :isRequired="true"
      />
      <Input
        inp_type="text"
        inp_name="name"
        @onUpdate="update"
        inp_placeholder="Имя"
        :min_length="2"
        :isRequired="true"
      />
      <Input
        inp_type="text"
        inp_name="second_name"
        @onUpdate="update"
        inp_placeholder="Отчество"
        :isRequired="false"
      />
      <Input
        inp_type="date"
        inp_name="birth"
        @onUpdate="update"
        inp_placeholder="Дата рождения"
        :isRequired="true"
      />
      <!--Дата рождения-->
      <Input
        inp_type="tel"
        inp_name="tel"
        @onUpdate="update"
        inp_placeholder="Телефон"
        :min_length="11"
        :max_length="11"
        special_format="70000000000"
        special_pattern="^7\d{10}"
        :isRequired="true"
      />
      <Select
        inp_name="sex"
        inp_placeholder="Пол"
        @onUpdate="update"
        :options="[
          { name: 'Мужской', value: 'Male' },
          { name: 'Женский', value: 'Famale' }
        ]"
        :isRequired="false"
      />

      <MultiSelect
        inp_name="group"
        :options="[
          { name: 'VIP', value: 'vip' },
          { name: 'Проблемные', value: 'problem' },
          { name: 'ОМС', value: 'oms' }
        ]"
        inp_placeholder="Группа пациентов"
        @onUpdate="update"
        :isRequired="true"
      />
      <Input
        inp_type="email"
        inp_name="email"
        @onUpdate="update"
        inp_placeholder="Email"
        :min_length="6"
        :isRequired="true"
      />
      <Select
        inp_name="пол"
        :options="[
          { name: 'Мужской', value: 'Male' },
          { name: 'Женский', value: 'Famale' }
        ]"
        inp_placeholder="Пол"
        :isRequired="false"
      />

      <Select
        inp_name="doctor"
        @onUpdate="update"
        inp_placeholder="Лечащий врач"
        :options="[
          { name: 'Иванов', value: 'Ivanov' },
          { name: 'Захаров', value: 'Zakhar' },
          { name: 'Чернышов', value: 'Chernysh' }
        ]"
        :isRequired="true"
      />
      <checkbox
        inp_name="sms"
        inp_placeholder="СМС уведомления"
        @onUpdate="update"
        onTrue="Уберите галочку, если не хотите получать от нас СМС"
        onFalse="Поставьте галочку, если не против получать от нас СМС"
      />

      <p>Адрес</p>
      <Input
        inp_type="number"
        inp_name="index"
        @onUpdate="update"
        inp_placeholder="Индекс"
        :min_length="5"
        :max_length="6"
        special_format="123123"
        :isRequired="false"
      />
      <Input
        inp_type="text"
        inp_name="country"
        @onUpdate="update"
        inp_placeholder="Страна"
        :isRequired="false"
      />
      <Input
        inp_type="text"
        inp_name="area"
        @onUpdate="update"
        inp_placeholder="Область"
        :isRequired="false"
      />
      <Input
        inp_type="text"
        inp_name="city"
        @onUpdate="update"
        inp_placeholder="Город"
        :isRequired="true"
      />
      <Input
        inp_type="text"
        inp_name="street"
        @onUpdate="update"
        inp_placeholder="Улица"
        :isRequired="false"
      />
      <Input
        inp_type="text"
        inp_name="building"
        @onUpdate="update"
        inp_placeholder="Дом"
        :isRequired="false"
      />

      <p>Документ</p>
      <Select
        inp_name="docType"
        @onUpdate="update"
        inp_placeholder="Тип документа"
        :options="[
          { name: 'Паспорт', value: 'Pass' },
          { name: 'Свидетельство о рождении', value: 'birthCert' },
          { name: 'Водительское удостоверение', value: 'driversLic' }
        ]"
        :isRequired="true"
      />
      <Input
        inp_type="text"
        inp_name="serial_number"
        @onUpdate="update"
        inp_placeholder="Серия"
        :isRequired="false"
      />
      <Input
        inp_type="text"
        inp_name="doc_number"
        @onUpdate="update"
        inp_placeholder="Номер документа"
        :isRequired="false"
      />
      <Input
        inp_type="text"
        inp_name="issued"
        @onUpdate="update"
        inp_placeholder="Кем выдан"
        :isRequired="false"
      />
      <Input
        inp_type="date"
        inp_name="issued_date"
        @onUpdate="update"
        inp_placeholder="Дата выдачи"
        :isRequired="true"
      />
      <!--Дата выдачи дока-->
      <button type="submit" @click="onSubmit">Submit</button>

      <p>{{ success_message }}</p>
    </form>
  </div>
</template>

<script>
import Input from "../components/Input.vue";
import Select from "../components/Selector.vue";
import Checkbox from "../components/Checkbox.vue";
import MultiSelect from "../components/MultiSelector.vue";

export default {
  name: "RegisterForm",
  data() {
    return {
      valid_form: false,
      success_message: ""
    };
  },
  methods: {
    update(data) {
      this.success_message = "";
      this.$data[data.name] = {
        isValid: data.isValid,
        val: data.inp,
        isRequired: data.isRequired
      };
    },
    onSubmit(event) {
      event.preventDefault();
      this.$children.forEach(item => {
        if (item.$v) {
          item.validate();
        }
      });
      this.valid_form = Object.values(this.$data)
        .filter(item => item.isRequired)
        .every(item => item.val.length > 0 && item.isValid);
      if (this.valid_form) {
        this.success_message = "Новый пациент создан";
        console.groupCollapsed("Данные, принятые от формы");
        console.info(this.$data);
        this.resetForm();
      }
      if (!this.valid_form)
        this.success_message = "Проверьте корректность заполнения полей";
    },
    resetForm() {
      this.$children.forEach(item => {
        item.refreshInput();
      });
    }
  },
  components: {
    Input,
    Select,
    Checkbox,
    MultiSelect
  }
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
.star {
  color: rgb(252, 119, 119);
}
span {
  margin: 0 0 15px 0;
}
p {
  font-weight: bolder;
  font-size: 25px;
  margin: 0 0 15px 0;
}
button {
  border-radius: 15px;
  background: #fff;
  outline: none;
  border: none;
  box-shadow: 0 0 3px #ccc;
  z-index: 0;
  padding: 10px;
  -webkit-box-shadow: 0 0px 3px #ccc;
  -webkit-appearance: none;
  cursor: pointer;
}
button:hover {
  transition: 0.3s linear;
  background: #ccc;
}
button:active {
  transition: 0.2s linear;
  background: rgb(126, 126, 126);
}
@media screen and(max-width: 530px) {
  .main_form {
    width: 70%;
  }
}
</style>
