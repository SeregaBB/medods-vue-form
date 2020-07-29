<template>
  <div id="app">
    <RegisterForm :inputs="inputs" />
  </div>
</template>

<script>
import RegisterForm from "./components/RegisterForm.vue";

export default {
  name: "App",
  data: () => { //дата должна быть функцией 
    return {

      inputs: { 
        personal: {
          //типы и плейсхолдеры для инпутов
        0: {type: 'email', plholder: 'Фамилия', isRequired: true}, 
        1: {type: 'alphaNum', plholder: 'Имя', isRequired: true}, 
        2: {type: 'alphaNum', plholder: 'Отчество', isRequired: false}, 
        3: {type: 'date', plholder: 'Дата рождения', isRequired: true},
        4: {type: 'tel', plholder: '+7(000)00-00-00', isRequired: true},  
        5: {type: 'alphaNum', plholder: 'Пол', isRequired: false}, //мультиселект
        6: {type: 'alphaNum', plholder: 'Группа пациентов', isRequired: true},  //заменить на мультиселект, подумать как это сделать во Вью
        7: {type: 'alphaNum', plholder: 'Лечащий врач', isRequired: false}, //Selector 
        8: {type: 'checkbox', plholder: '', isRequired: false},  
        },
        adress: {
        9: {type: 'number', plholder: 'Индекс', isRequired: false},
        10: {type: 'alphaNum', plholder: 'Страна', isRequired: false},
        11: {type: 'alphaNum', plholder: 'Область', isRequired: false},
        12: {type: 'alphaNum', plholder: 'Город', isRequired: true},
        13: {type: 'alphaNum', plholder: 'Улица', isRequired: false},
        14: {type: 'alphaNum', plholder: 'Дом', isRequired: false},
        },
        documents: {
        15: {type: 'alphaNum', plholder: 'Тип документа', isRequired: true},//selector
        16: {type: 'number', plholder: 'Серия', isRequired: false},
        17: {type: 'number', plholder: 'Номер', isRequired: false},
        18: {type: 'alphaNum', plholder: 'Кем выдан', isRequired: false},
        19: {type: 'date', plholder: 'дата выдачи', isRequired: true},
        }
      }
    };
  },
  components: {
    RegisterForm
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #15202b;
  margin: 0;
  width: 100vw;
  background-image: url('./assets/back.jpg');
  padding: 20px 0;
}
body{ 
  margin: 0
}
</style>
