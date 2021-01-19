<template>
  <main class='main'>
    <div class='table'>
      <button class='add-user' @click='showModal'>Добавить</button>
      <p v-if='users.length === 0'>Добавьте пользователя</p>
      <table v-else>
        <thead>
          <tr>
            <th @click="sortParam='name'">Имя</th>
            <th @click="sortParam='phone'">Телефон</th>
          </tr>
        </thead>
        <tbody v-for='(user, index) in sortedList' :key='index'>
          <tr>
            <td class='user-name'>
              <span
                v-if='user.subusers && user.subusers.length > 0'
                @click='user.isSubUsersShow = !user.isSubUsersShow'
                class='show-subuser'>
              </span>
              {{ user.name }}
            </td>
            <td>{{ user.phone }}</td>
          </tr>
          <template v-if='user.isSubUsersShow'>
            <tr v-for='(subuser, index) in user.subusers' :key='index' class='subuser'>
              <td class='subuser-name'>{{ subuser.name }}</td>
              <td>{{ subuser.phone }}</td>
          </tr>
          </template>
        </tbody>
      </table>
    </div>
    <transition name='modal'>
      <Modal
        v-if='isModalShown'
        @closeModal='closeModal'
        @addUser='addUser'
        :users='users'
      />
    </transition>
  </main>
</template>

<script>
import Modal from './Modal'

export default {
  name: 'Table',
  components: {
    Modal
  },
  data () {
    return {
      users: [],
      sortParam: null,
      isModalShown: false
    }
  },
  mounted () {
    if (localStorage.getItem('users')) {
      try {
        this.users = JSON.parse(localStorage.getItem('users'))
      } catch (e) {
        localStorage.removeItem('users')
      }
    }
  },
  methods: {
    showModal () {
      this.isModalShown = true
    },
    closeModal () {
      this.isModalShown = false
    },
    addUser (data) {
      if (data.chief !== null) {
        this.users[data.chief].subusers.push({
          name: data.name,
          phone: data.phone
        })
      } else {
        this.users.push({
          name: data.name,
          phone: data.phone,
          subusers: [],
          isSubUsersShow: false
        })
      }
      localStorage.setItem('users', JSON.stringify(this.users))
      this.isModalShown = false
    }
  },
  computed: {
    sortedList () {
      switch (this.sortParam) {
        case 'name': return this.users.slice().sort((a, b) => (a.name.toLowerCase() > b.name.toLowerCase()) ? 1 : -1)
        case 'phone': return this.users.slice().sort((a, b) => (a.phone > b.phone) ? 1 : -1)
        default: return this.users
      }
    }
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
  .main {
    display: flex;
    max-width: 1024px;
    margin: 0 auto;
  }
  .main > div {
    width: calc(50% - 25px);
    position: relative;
  }
  .main > div:first-child {
    margin-right: 50px;
  }
  table {
    border-collapse: collapse;
    width: 100%;
    text-align: left;
  }
  th, td {
    width: 50%;
    min-height: 39px;
    border: 1px solid #000;
    padding: 10px;
  }
  th {
    cursor: pointer;
  }
  .add-user {
    position: absolute;
    top: -50px;
    right: 0;
  }
  .user-name, .subuser-name {
    padding-left: 18px;
  }
  .subuser-name {
    display: flex;
    width: 90%;
    margin-left: auto;
    border-right: none;
    border-top: none;
  }
  .show-subuser {
    display: inline-block;
    background: #000;
    height: 10px;
    width: 2px;
    position: relative;
    left: -8px;
    cursor: pointer;
  }
  .show-subuser:after {
    content: "";
    height: 2px;
    width: 10px;
    background: #000;
    position: absolute;
    left: -4px;
    top: 4px;
  }
  /*Animation*/
  .modal-enter-active, .modal-leave-active {
    transition: all .5s;
  }
  .modal-enter, .modal-leave-to {
    opacity: 0;
  }
  @media (max-width: 768px) {
    .main {
      position: relative;
    }
    .main > div {
      width: 100%;
    }
    .main > div:first-child {
      margin-right: 0;
    }
  }
</style>
