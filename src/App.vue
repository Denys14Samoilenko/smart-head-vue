<script lang="ts">
import { User } from './types/User.ts'
import Card from './components/Card/Card.vue'
import AddUserForm from './components/AddUserForm/AddUserForm.vue'
import UserInfo from './components/UserInfo/UserInfo.vue'

export default {
   components: {
      Card,
      AddUserForm,
      UserInfo
   },

   data() {
      return {
         api_url: "https://reqres.in/api/users/",
         users: [] as User[],
         query: '',
         user: {} as User | undefined,
         isMenuOpen: false,
      }
   },

   mounted() {
      this.fetchUsers()
   },
   methods: {
      fetchUsers() {
         fetch(`${this.api_url}`)
            .then(res => res.json())
            .then(this.setUsers)
            .catch(error => {
               console.error('Error adding user:', error);
            });
      },
      deleteUser(userId: number) {
         fetch(`${this.api_url}${userId}`, {
            method: 'DELETE',
         })
            .then(() => {
               this.users = this.users.filter((user) => user.id !== userId);
            })
            .catch((error) => {
               console.error('Error deleting user:', error);
            });
      },
      setUsers(res: any) {
         this.users = res.data
      },
      addUser(user: User) {
         this.users.unshift(user)
      },
      showUserInfo(userId: number) {
         this.user = this.users.find((user: User) => user.id === userId)
      },
      updateUser(updatedUser: User | undefined) {
         let currentUser = this.users.find((user) => user.id === updatedUser?.id)
         if (currentUser) {
            currentUser.phone = updatedUser?.phone ?? '';
            currentUser.address = updatedUser?.address ?? '';
         }
      },
      menuOpen() {
         this.isMenuOpen = !this.isMenuOpen
      }
   },
   computed: {
      filteredUsers() {
         return this.users.filter(user =>
            `${user.first_name} ${user.last_name}`.toLowerCase().includes(this.query.toLowerCase())
         );
      },
   },
   watch: {
      isMenuOpen(newValue) {
         document.body.classList.toggle('menu-open', newValue);
      },
   },
}

</script>

<template >
   <header class="header">
      <input class="filter" type="text" v-model="query" placeholder="Enter user name">
   </header>

   <UserInfo @is-modal-open="menuOpen" @update-user="updateUser" :isMenuOpen=isMenuOpen v-if="isMenuOpen" :user="user" />
   <!-- v-if="user && Object.keys(user).length !== 0" -->
   <main class="main">
      <AddUserForm @add-user-submitted="addUser" />

      <div class="users" v-if="users.length">
         <ul class="users_list">
            <li class="users_item" :key="user.id" v-for="user in filteredUsers">
               <Card @is-modal-open="menuOpen" :isMenuOpen=isMenuOpen :user="user" @delete-user="deleteUser"
                  @show-user-info="showUserInfo" />
            </li>
         </ul>
      </div>
      <p v-if="!filteredUsers.length">Users not found</p>
   </main>
</template>

<style scoped lang="scss">
.users {

   &_list {
      list-style: none;
      margin: 0;
      padding: 0;
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 18px;
      max-width: 1024px;
   }

   &_item {
      width: 100%;
      border: 1px solid #000;
      border-radius: 8px;
      transition: background-color 0.5s, box-shadow 0.5s;
      height: 100%;

      &:hover {
         background-color: yellow;
         color: #fff;
         box-shadow: 0px 0px 15px #000;
      }
   }
}

.filter {
   padding: 8px 12px;
   border-radius: 4px;
   outline: none;
   border: 1px solid #000;
   font-size: 16px;
   height: 40px;
   width: 60%;
   transition: all 0.5s;

   &:focus {
      background-color: #e5e5e5;
      box-shadow: 0px 0px 8px #000;
      width: 80%;
   }
}
</style>
