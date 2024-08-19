<script lang="ts">
import { User } from '../../types/User'

export default {
   props: {
      user: {
         type: Object as () => User | undefined,
         required: true,
      },
      isMenuOpen: {
         type: Boolean,
         required: true,
      }
   },
   data() {
      return {
         phone: this.user?.phone,
         address: this.user?.address,
         isUpdated: false,
         updatedUser: { ...this.user },
      }
   },
   methods: {
      closeModal() {
         this.$emit('update-user', this.updatedUser);
         this.$emit('is-modal-open', true);
         this.isUpdated = false
      },

      setIsUpdated() {
         this.isUpdated = true
      },

      saveInfo() {
         this.updatedUser.phone = this.phone
         this.updatedUser.address = this.address
         this.isUpdated = false
      },
   },
   computed: {
      getUserEmailPath() {
         return 'mailto:' + this.user?.email;
      },

      getFullName() {
         return `${this.user?.first_name} ${this.user?.last_name}`.trim();
      },
   }
}
</script>

<template>
   <div class="modal">
      <div class="modal_content ">
         <img class="modal_avatar" :src="user?.avatar" alt="User avatar">

         <div class="modal_info">
            <h2 class="modal_name">Full name: {{ getFullName }}</h2>
            <span>Email: </span>
            <a class="" :href="getUserEmailPath">{{ user?.email }}</a>

            <form class="modal_form">
               <label for=""> Phone:
                  <input class="modal_input" v-if="isUpdated" type="text" v-model="phone">
                  <p v-else>{{ updatedUser?.phone }}</p>
               </label>

               <label for=""> Address:
                  <input class="modal_input" v-if="isUpdated" type="text" v-model="address">
                  <p v-else>{{ updatedUser?.address }}</p>
               </label>
            </form>

            <div class="modal_buttons">
               <button v-if="!isUpdated" class="modal_button" @click="setIsUpdated">&#9998;</button>
               <button v-else class="modal_button" @click="saveInfo">Save</button>
               <button class="modal_button" @click="closeModal">X</button>
            </div>
         </div>
      </div>
   </div>
</template>

<style scoped lang="scss">
.modal {
   position: fixed;
   inset: 0;
   display: flex;
   align-items: center;
   justify-content: center;
   background-color: rgba($color: #000000, $alpha: 0.7);
	color: #000000;

   &_content {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      background-color: #fff;
      padding: 1rem;
      border-radius: 5px;
      margin: 16px;
   }

   &_info {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      word-break: break-all;
   }

   &_button {
      transition: all 0.5s;

      &:hover {
         background-color: #646cff;
         color: #fff;
      }
   }

   &_buttons {
      display: flex;
      justify-content: center;
      gap: 20px;
   }

   &_avatar {
      margin: 0 auto;
      border-radius: 8px;
      width: 150px;
      object-fit: contain;
   }

   &_form {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
      padding: 10px 0;
   }

   &_input {
      padding: 8px;
      border-radius: 5px;
		background-color: #fff;
		color: #000000;
   }

}
</style>