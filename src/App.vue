<template>
  <div class="wrapper">
    <div v-if="state.show">
      <div class="content">
        <form class="registration-form" @submit.prevent="onSubmit">
          <div class="form-part">
            <p>
              <label for="name" data-end="*">Имя</label>
              <input id="name" v-model="state.name" placeholder="Имя">
              <span v-if="v$.name.$error">{{ v$.name.$errors[0].$message }}</span>
            </p>
            <p>
              <label for="lastName" data-end="*">Фамилия</label>
              <input id="lastName" v-model="state.lastName" placeholder="Фамилия">
              <span v-if="v$.lastName.$error">{{ v$.lastName.$errors[0].$message }}</span>
            </p>
            <p>
              <label for="patronymicName">Отчество</label>
              <input id="patronymicName" v-model="patronymicName" placeholder="Отчество">
            </p>
            <p>
              <label for="birthDate" data-end="*">Дата рождения</label>
              <input id="birthDate" v-model="state.birthDate" placeholder="birthDate" type="date">
              <span v-if="v$.birthDate.$error">{{ v$.birthDate.$errors[0].$message }}</span>
            </p>
            <p>
              <label for="phoneNumber" data-end="*">Номер телефона</label>
              <input id="phoneNumber" v-model="state.phoneNumber" placeholder="Номер телефона" type="tel">
              <span v-if="v$.phoneNumber.$error">{{ v$.phoneNumber.$errors[0].$message }}</span>
            </p>
            <div class="sex">
              <label>Выберите пол: </label>
              <p>
                <label>Муж</label>
                <input value="male" v-model="sex" type="radio">
                <label>Жен</label>
                <input value="famale" v-model="sex" type="radio">
              </p>
            </div>
            <p>
              <label data-end="*">Группа клиентов</label>
              <label>Выбраны: {{ state.clientsGroup }}</label>
              <select v-model="state.clientsGroup" multiple>
                <option>VIP</option>
                <option>Проблемные</option>
                <option>ОМС</option>
              </select>
              <span v-if="v$.clientsGroup.$error">{{ v$.clientsGroup.$errors[0].$message }}</span>
            </p>
            <p>
              <label>Лечащий врач</label>
              <select v-model="therapist">
                <option disabled value="">Выберите один из вариантов</option>
                <option>Иванов</option>
                <option>Захаров</option>
                <option>Чернышева</option>
              </select>
            </p>
            <p class="noSms">
              <label for="noSMS">Не отправлять СМС</label>
              <input id="noSMS" v-model="noSMS" type="checkbox">
            </p>
          </div>
          <div class="form-part">
            <p>
              <label for="postIndex">Индекс</label>
              <input id="postIndex" v-model="state.postIndex" placeholder="Индекс">
              <span v-if="v$.postIndex.$error">{{ v$.postIndex.$errors[0].$message }}</span>
            </p>
            <p>
              <label for="country">Страна</label>
              <input id="country" v-model="country" placeholder="Страна">
            </p>
            <p>
              <label for="region">Область</label>
              <input id="region" v-model="region" placeholder="Область">
            </p>
            <p>
              <label for="city" data-end="*">Город</label>
              <input id="city" v-model="state.city" placeholder="Город">
              <span v-if="v$.city.$error">{{ v$.city.$errors[0].$message }}</span>
            </p>
            <p>
              <label for="street">Улица</label>
              <input id="street" v-model="street" placeholder="Улица">
            </p>
            <p>
              <label for="houseNumber">Дом</label>
              <input id="houseNumber" v-model="houseNumber" placeholder="Дом" type="number">
            </p>
          </div>
          <div class="form-part">
            <p>
              <label data-end="*">Тип документа</label>
              <select v-model="state.documentType">
                <option disabled value="">Выберите один из вариантов</option>
                <option>Паспорт</option>
                <option>Свидетельство о рождении</option>
                <option>Вод. удостоверение</option>
              </select>
              <span v-if="v$.documentType.$error">{{ v$.documentType.$errors[0].$message }}</span>
            </p>
            <p>
              <label for="documentSeries">Серия</label>
              <input id="documentSeries" v-model="state.documentSeries" placeholder="Серия">
              <span v-if="v$.documentSeries.$error">{{ v$.documentSeries.$errors[0].$message }}</span>
            </p>
            <p>
              <label for="documentNumber">Номер</label>
              <input id="documentNumber" v-model="state.documentNumber" placeholder="Номер">
              <span v-if="v$.documentNumber.$error">{{ v$.documentNumber.$errors[0].$message }}</span>
            </p>
            <p>
              <label for="documentIssued">Кем выдан</label>
              <input id="documentIssued" v-model="documentIssued" placeholder="Кем выдан">
            </p>
            <p>
              <label for="documentDate" data-end="*">Дата выдачи</label>
              <input id="documentDate" v-model="state.documentDate" type="date">
              <span v-if="v$.documentDate.$error">{{ v$.documentDate.$errors[0].$message }}</span>
            </p>
          </div>
        </form>
        <div class="submit">
          <button @click="onSubmit">submit</button>
        </div>
        <p>*Поле обязательное для заполнения.</p>
      </div>
    </div>
    <div v-else>
      <div class="content">
        <div class="formOK"><span>Форма регистрации отправлена</span></div>
      </div>
    </div>
  </div>
</template>


<script>
import useValidate from '@vuelidate/core'
import {required, minLength,maxLength, numeric} from '@vuelidate/validators'
import {reactive, computed} from 'vue'


export default {
  name: 'App',

  setup() {
    const state = reactive({
      name: '',
      lastName: '',
      phoneNumber: '7',
      clientsGroup: '',
      birthDate: null,
      city: null,
      documentType: null,
      documentSeries:null,
      documentNumber:null,
      documentDate: null,
      postIndex:null,
      sex: null,
      show: true
    })

    const rules = computed(() => {
      return {
        name: {required, minLength: minLength(2)},
        lastName: {required, minLength: minLength(2)},
        phoneNumber: {required, minLength: minLength(11), maxLength:maxLength(11),numeric},
        clientsGroup: {required},
        birthDate: {required},
        city: {required},
        postIndex:{numeric},
        documentType: {required},
        documentSeries:{numeric},
        documentNumber:{numeric},
        documentDate: {required}

      }
    })
    const v$ = useValidate(rules, state)
    return {
      state,
      v$
    }
  },
  watch: {
    phoneNumber: function (newNumber) {
      if (newNumber.leave < 10) {
        this.phoneNumber = '+7'
      }
    }
  },

  methods: {
    onSubmit() {
      this.v$.$validate()
      if (!this.v$.$error) {
        this.state.show = !this.state.show
        let personData = {
          name: this.state.name,
          lastName: this.state.lastName,
          patronymicName: this.patronymicName,
          birthDate: this.state.birthDate,
          phoneNumber: this.state.phoneNumber,
          sex: this.sex,
          therapist: this.therapist,
          noSMS: this.noSMS,
          clientsGroup: this.state.clientsGroup,

          postIndex: this.state.postIndex,
          country: this.country,
          region: this.region,
          city: this.state.city,
          street: this.street,
          houseNumber: this.houseNumber,

          documentType: this.state.documentType,
          documentSeries: this.state.documentSeries,
          documentNumber: this.state.documentNumber,
          documentIssued: this.documentIssued,
          documentDate: this.state.documentDate
        }
        console.log(personData)
      } else {
        alert('Form is incorrect!!!')
      }
    }
  }
}

</script>
