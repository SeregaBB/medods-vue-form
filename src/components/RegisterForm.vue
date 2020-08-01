<template>
  <div class="register">
    <form class="main_form" name="new" >
      <span class="form_block_title">Персональная информация</span>
      <input
        id="0"
        type="text"
        placeholder="Фамилия (Обязательно)"
        v-model.trim="lastName"
        @input="$v.lastName.$touch()"
        :class="{invalid: ($v.lastName.$error)}"
        required
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
      <label for="birth">Дата рождения (обязательно)</label>
      <input id="3" name="birth" type="date"
      v-model="birthday"
      @input="$v.birthday.$touch()"
      :class="{invalid: (!$v.birthday.$required && $v.birthday.$dirty && $v.birthday.$error)}"
      />
      <small class="error" v-if="(!$v.birthday.$required && $v.birthday.$dirty && $v.birthday.$error)">Введите дату рождения</small>
      <input id="4" type="number" placeholder="7000000000 (Обязательно)" 
      v-model="tel"
      @input="$v.tel.$touch()"
      :class="{invalid: ($v.tel.$error)}"
      />
      <small
        class="error"
        v-if="$v.tel.$dirty && !$v.tel.$minLength && $v.tel.required && (Number(tel.slice(0,1)) === 7) && $v.tel.$error"
      >Телефон должен состоять из 11 цифр (вы ввели {{tel.length}})</small>
      <small
        class="error"
        v-else-if="$v.tel.$dirty && (Number(tel.slice(0,1)) !== 7) && (tel.length > 0)"
      >Телефон должен начинаться с 7</small>
      <small
        class="error"
        v-else-if="$v.tel.$dirty && !$v.tel.required"
      >Введите телефон</small>
       <select
        v-model="sex"
      >
      <option disabled hidden >Пол</option>
        <option>Мужской</option>
        <option>Женский</option>
        <option>Другой</option>
       </select>
     
      <select multiple
        v-model="group"
        :class="[{invalid: ($v.group.$dirty && $v.group.$model[0] === 'Группа пациентов (обязательно)' && $v.group.$model.length === 1)}]"
      >
        <option disabled >Группа пациентов (обязательно)</option>
        <option >Vip</option>
        <option >Проблемные</option>
        <option >ОМС</option>
      </select>

      <small
        class="error"
        v-if="$v.group.$dirty && $v.group.$model[0] === 'Группа пациентов (обязательно)' && $v.group.$model.length === 1"
      >Выберите группу пациентов</small>

      <select
        v-model="doctor"
        
      >
        <option disabled hidden>Лечащий врач</option>
        <option>Иванов</option>
        <option>Захаров</option>
        <option>Чернышева</option>
      </select>


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
      name="doctype"
        v-model="doctype"
        @click="$v.doctype.$touch()"
        :class="{invalid: ($v.doctype.$dirty && $v.doctype.$model === 'Тип документа (обязательно)')}"
      >
      
        <option disabled hidden >Тип документа (обязательно)</option>
        <option>Паспорт</option>
        <option>Свидетельство о рождении</option>
        <option>Вод. удостоверение</option>
      </select>
      <small
        class="error"
        v-if="$v.doctype.$dirty && $v.doctype.$model === 'Тип документа (обязательно)'"
      >Выберите тип документа</small>

      <input type="number" placeholder="Серия " />
      <input type="number" placeholder="Номер " />
      <input type="text" placeholder="Кем выдан " />
      <label for="docdate">Дата выдачи (обязательно)</label>
      <input type="date" name="docdate"
      v-model="docdate"
      @input="$v.docdate.$touch()"
      :class="{invalid: (!$v.docdate.$required && $v.docdate.$dirty && $v.docdate.$error)}"
      />
      <small class="error" v-if="(!$v.docdate.$required && $v.docdate.$dirty && $v.docdate.$error)">Введите дату выдачи документа</small>


      <button type="button" v-if="this.$v.$anyError"  disabled>Сохранить</button>
      <button type="button" v-else @click="onSubmit" >Сохранить</button>
      <span>{{ success }}</span>


    </form>
  </div>
</template>

<script>
import { helpers, required, minLength } from "vuelidate/lib/validators";
const alpha = helpers.regex("alpha", /^[а-яА-Яa-zA-Z]*$/);
const telValodator = helpers.regex('telValidator',/^7\d{10}/gi);

const defaultData = ()=>({
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
    success: '', 
  })
export default {
  name: "RegisterForm",
  data: () => defaultData(),
  validations: {
    lastName: { alpha, required, minLength: minLength(2) },
    name: { alpha, required, minLength: minLength(2) },
    birthday: {required},
    group: { required },
    sms: false,
    city: { required, minLength: minLength(3), alpha },
    doctype: {required}, 
    docdate: {required},
    tel: {required, telValodator, minLength: minLength(11)}
  },
  
  ///методы
  methods: {
    
    resetData(){
      Object.assign(this.$data, defaultData())
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
	width: 40%;
	select {
		>option {
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
		font-size: 30px;
		font-weight: bolder;
		text-shadow: 0 3px 3px white;
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
