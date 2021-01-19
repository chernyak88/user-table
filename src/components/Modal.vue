<template>
  <div class='modal'>
    <form>
      <span class='close' @click='closeModal'></span>
      <legend>Добавление пользователя</legend>
      <div>
        <label for='name'>Имя</label>
        <input
          v-model='name'
          type='text'
          name='name'
          placeholder='Иван'
          @keypress='isLetter($event)'
        >
      </div>
      <div>
        <label for='phone'>Телефон</label>
        <phone-mask-input
          v-model="phone"
          name='phone'
          autoDetectCountry
        />
      </div>
      <div v-if='!isEmpty'>
        <label for='chief'>Начальник</label>
        <select v-model='chief' name='chief'>
          <option disabled selected>Выберите из списка</option>
          <option v-for='(user, index) in users' :key='index' :value='index'>{{ user.name }}</option>
        </select>
      </div>
      <button class='save' type='submit' @click='addUser'>Сохранить</button>
      <div class="tooltip" v-if="IsTooltipShow">
        Поля "Имя" и "Телефон" должны быть заполнены
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  data () {
    return {
      name: null,
      phone: null,
      chief: null,
      isEmpty: null,
      IsTooltipShow: false
    }
  },
  props: {
    users: Array
  },
  mounted () {
    const isEmpty = x => !Object.keys(x).length
    if (isEmpty(this.users[0])) {
      this.isEmpty = true
    } else {
      this.isEmpty = false
    }
  },
  methods: {
    closeModal () {
      this.$emit('closeModal')
    },
    addUser (event) {
      event.preventDefault()
      this.$emit('addUser', {
        name: this.name,
        phone: this.phone,
        chief: this.chief
      })
    },
    isLetter (event) {
      let char = String.fromCharCode(event.keyCode)
      if (/^[а-яА-Яa-zA-Zё]+$/.test(char)) {
        return true
      } else {
        event.preventDefault()
      }
    }
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
  form {
    position: relative;
    max-width: 320px;
    border: 1px solid #000;
    padding: 10px 20px 30px 20px;
    text-align: left;
  }
  form legend, form > div {
    line-height: 25px;
    margin-bottom: 30px;
  }
  form > div {
    display: flex;
    justify-content: space-between;
  }
  input, select {
    width: 180px;
    height: 25px;
  }
  .close {
    position: absolute;
    right: 20px;
    top: 15px;
    width: 16px;
    height: 16px;
    opacity: 0.3;
    cursor: pointer;
  }
  .close:hover {
    opacity: 1;
  }
  .close:before, .close:after {
    position: absolute;
    left: 8px;
    content: ' ';
    height: 16px;
    width: 2px;
    background-color: #333;
  }
  .close:before {
    transform: rotate(45deg);
  }
  .close:after {
    transform: rotate(-45deg);
  }
  .tooltip {
    position: absolute;
    left: 20px;
    bottom: -45px;
    width: 150px;
    font-size: 12px;
    line-height: 14px;
    background: red;
    color: #ffffff;
    text-align: center;
    padding: 5px;
    border-radius: 0;
  }
  .tooltip::after {
    content: '';
    position: absolute;
    left: 65px;
    top: -15px;
    border: 10px solid transparent;
    border-bottom: 10px solid red;
   }
  @media (max-width: 768px) {
    div.modal {
      position: absolute;
      top: 0;
      left: 0;
    }
    form {
      background: #fff;
      margin: auto;
    }
  }
</style>
