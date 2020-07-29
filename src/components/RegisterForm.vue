<template>
  <div class="register">
    <form class="main_form" name="new" @input="onInp" @click="onInp">
      <span class="form_block_title">Персональная информация</span>
      <input
        id="0"
        type="text"
        placeholder="Фамилия (Обязательно)"
        v-model.trim="lastName"
        @input="$v.lastName.$touch()"
        :class="{invalid: ($v.lastName.$error)}"
      />
      <small class="error" v-if="!$v.lastName.minLength">Фамилия не может состоять из 1 буквы</small>
      <small class="error" v-else-if="!$v.lastName.alpha">Фамилия должна состоять только из букв</small>
      <small class="error" v-else-if="$v.lastName.$dirty && !$v.lastName.required">Введите фамилию</small>

      <input
        id="1"
        type="text"
        placeholder="Имя (Обязательно)"
        v-model.trim="name"
        @input="$v.name.$touch()"
        :class="{invalid: ($v.name.$error)}"
      />

      <small class="error" v-if="!$v.name.minLength">Имя не может состоять из 1 буквы</small>
      <small class="error" v-else-if="!$v.name.alpha">Имя должно состоять только из букв</small>
      <small class="error" v-else-if="$v.name.$dirty && !$v.name.required">Введите имя</small>

      <input id="2" type="text" placeholder="Отчество " />
      <input id="3" type="date" placeholder="Дата рождения (Обязательно)" />
      <input id="4" type="tel" placeholder="+7(000)00-00-00 (Обязательно)" />
      <input id="5" type="text" placeholder="Пол " />
      <!--Это должен быть мультиселект-->
      <select multiple
        v-model="group"
        @click="$v.group.$touch()"
        :class="{invalid: ($v.group.$dirty && $v.group.$model === 'Группа пациентов')}"
      >
        <option disabled >Группа пациентов</option>
        <option>Vip</option>
        <option>Проблемные</option>
        <option>ОМС</option>
      </select>

      <small
        class="error"
        v-if="$v.group.$dirty && $v.group.$model === 'Группа пациентов'"
      >Выберите группу пациентов</small>

      <select
        v-model="doctor"
        @click="$v.doctor.$touch()"
        :class="{invalid: ($v.doctor.$dirty && $v.doctor.$model === 'Лечащий врач')}"
      >
        <option disabled >Лечащий врач</option>
        <option>Иванов</option>
        <option>Захаров</option>
        <option>Чернышева</option>
      </select>

      <small
        class="error"
        v-if="$v.doctor.$dirty && $v.doctor.$model === 'Лечащий врач'"
      >Выберите врача</small>


      <div class="group">
        <input id="8" name="sms" type="checkbox" v-model="sms" />
        <label v-if="!$v.sms.$model" for="sms">Не отправлять мне СМС</label>
        <label v-else for="sms">СМС не будут приходить</label>
      </div>

      <span class="form_block_title">Адрес</span>
      <input type="number" placeholder="Индекс " />
      <input type="text" placeholder="Страна " />
      <input type="text"  placeholder="Область " />

      <input
        type="text"
        placeholder="Город (Обязательно)"
        v-model.trim="city"
        @input="$v.city.$touch()"
        :class="{invalid: ($v.city.$error)}"
      />
      <small
        class="error"
        v-if="!$v.city.minLength"
      >Название города не может состоять из менее, чем 3 букв</small>
      <small class="error" v-else-if="!$v.city.alpha">Название города должно состоять только из букв</small>
      <small class="error" v-else-if="$v.city.$dirty && !$v.city.required">Введите название города</small>

      <input type="text" placeholder="Улица " />
      <input type="text" placeholder="Дом " />
      <span class="form_block_title">Документ</span>

      <select
        v-model="doctype"
        @click="$v.doctype.$touch()"
        :class="{invalid: ($v.doctype.$dirty && $v.doctype.$model === 'Тип документа')}"
      >
        <option disabled >Тип документа</option>
        <option>Паспорт</option>
        <option>Свидетельство о рождении</option>
        <option>Вод. удостоверение</option>
      </select>
      <small
        class="error"
        v-if="$v.doctype.$dirty && $v.doctype.$model === 'Тип документа'"
      >Выберите тип документа</small>

      <input type="number" placeholder="Серия " />
      <input type="number" placeholder="Номер " />
      <input type="text" placeholder="Кем выдан " />
      <input type="date" placeholder="дата выдачи (Обязательно)" />


      <button type="button" v-if="this.$v.$anyError"  disabled>Сохранить</button>
      <button type="button" v-else @click="onSubmit" >Сохранить</button>
      <span>{{ success }}</span>


    </form>
  </div>
</template>

<script>
import { helpers, required, minLength } from "vuelidate/lib/validators";
const alpha = helpers.regex("alpha", /^[а-яА-Яa-zA-Z]*$/);

const defaultData = ()=>({
    lastName: "",
    name: "",
    birthday: "",
    tel: "",
    group: ["Группа пациентов"],
    city: "",
    doctor: "Лечащий врач",
    doctype: "Тип документа",
    docdate: "",
    sms: false,
    success: '', 
  })
export default {
  name: "RegisterForm",
  //props: ["inputs"],
  ///vuelidate

  data: () => defaultData(),
  validations: {
    lastName: { alpha, required, minLength: minLength(2) },
    name: { alpha, required, minLength: minLength(2) },
    group: { required },
    doctor: { required },
    sms: false,
    city: { required, minLength: minLength(3), alpha },
    doctype: {required}
  },
  
  ///методы
  methods: {

    resetData(){
      Object.assign(this.$data, defaultData())
    },
    onInp(){
      console.log(this.$v);
    },
//при сабмите формы забираем все введённые данные в форму + выводим сообщение "новый клиент успешно создан"
    onSubmit() {
      const formData = {};
      this.$v.$touch();
       if(!this.$v.$anyError) {
         
        
         for (let key in this.$v) { 
           if(!key.includes('$')) {
             formData[key] = this.$v[key].$model;
           }
         }
         console.log(formData);
         this.$v.$reset();
         this.resetData();
         
         this.success = 'Новый клиент успешно создан'; 
         setTimeout(()=>{
            this.success = '';
         }, 3000);
         
       }
    }
  }
  //конец методов
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
}

.main_form {
  width: 40%;
  
  input,
  select {
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
    
    &.invalid {
      box-shadow: 0 4px 5px rgb(255, 129, 129);
      -webkit-box-shadow: 0 4px 5px rgb(255, 129, 129);
      -webkit-appearance: none;
    }
    &[type="checkbox"] {
      align-self: flex-start;
      -webkit-appearance: checkbox;
    }
    &[type="date"] {
      width: 100%;
    }
  }
  span{ 
    width: 100%;
  }
  small {
    color: rgb(255, 129, 129);
    align-self: start;
    font-weight: bold;
    width: 100%;
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
span {
  width: 100%;
  font-size: 30px;
  font-weight: bolder;
  text-shadow: 0 3px 3px white;
}

@media screen and(max-width: 530px) {
      .main_form {
        width: 70%;
      }
}
</style>
