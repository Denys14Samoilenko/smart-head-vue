<script lang="ts">

export default {
   data() {
      return {
         api_url: "https://reqres.in/api/users",
         first_name: '',
         last_name: '',
         email: '',
         avatar: 'https://png.pngtree.com/png-vector/20220608/ourlarge/pngtree-user-profile-character-faceless-unknown-png-image_4816132.png',
         errors: {
            first_name: '',
            last_name: '',
            email: '',
         } as Record<string, string>,
      }
   },
   methods: {
      onSubmit() {
         this.clearErrors()

         let addUser = {
            first_name: this.first_name,
            last_name: this.last_name,
            email: this.email,
            avatar: this.avatar
         }

         fetch(`${this.api_url}`, {
            method: 'POST',
            headers: {
               'Content-Type': 'application/json',
            },
            body: JSON.stringify(addUser),
         })
            .then(response => response.json())
            .then(data => {
               this.$emit('add-user-submitted', data)
            })
            .catch(error => {
               console.error('Error adding user:', error);
            });

         this.clearForm()
      },

      isValid(value: string, regex: RegExp, errorField: string) {
         if (!regex.test(value)) {
            this.errors[errorField] = `Invalid ${errorField.replace('_', ' ')}`;
         } else {
            this.errors[errorField] = '';
         }
      },

      validateFirstName() {
         this.isValid(this.first_name, /^[a-zA-Z]+$/, 'first_name');
      },

      validateLastName() {
         this.isValid(this.last_name, /^[a-zA-Z]+$/, 'last_name');
      },

      validateEmail() {
         this.isValid(this.email, /^[^\s@]+@[^\s@]+\.[^\s@]+$/, 'email');
      },

      clearErrors() {
         for (const field in this.errors) {
            this.errors[field] = '';
         }
      },

      clearForm() {
         this.first_name = '';
         this.last_name = '';
         this.email = ''
      },
   },
   computed: {
      isSubmitDisabled() {
         return !!this.errors.first_name || !!this.errors.last_name || !!this.errors.email || !this.first_name || !this.last_name || !this.email;
      }
   }
}
</script>

<template>
   <form class="form" @submit.prevent="onSubmit">
      <label class="form_label" for="first_name">
         <input class="form_input" type="text" id="first_name" min="1" placeholder="First Name" v-model="first_name"
            @blur="validateFirstName">
         <span class="error-message" :class="{ 'visible': errors.first_name !== '' }">{{ errors.first_name }}</span>
      </label>

      <label class="form_label" for="last_name">
         <input class="form_input" type="text" id="last_name" placeholder="Last Name" v-model="last_name"
            @blur="validateLastName">
         <span class="error-message" :class="{ 'visible': errors.last_name !== '' }">{{ errors.last_name }}</span>
      </label>

      <label class="form_label" for="email">
         <input class="form_input" type="email" id="email" placeholder="Email" v-model="email" @blur="validateEmail">
         <span class="error-message" :class="{ 'visible': errors.email !== '' }">{{ errors.email }}</span>
      </label>

      <input class="form_button" type="submit" value="Add user" :disabled="isSubmitDisabled">
   </form>
</template>


<style scoped lang="scss">
.form {
   display: flex;
   flex-direction: column;
   flex-wrap: wrap;
   gap: 10px;
   padding: 1rem;

   @media (min-width:640px) {
      flex-direction: row;
      justify-content: center;
   }

   &_label {
      display: flex;
      flex-direction: column;
      text-align: start;
   }

   &_input {
      padding: 8px 12px;
      border-radius: 4px;
      outline: none;
      border: 1px solid #000;
      font-size: 16px;
      height: 40px;

      &:focus {
         background-color: #e5e5e5;
         box-shadow: 0px 0px 8px #000;
      }
   }

   &_button {
      height: 40px;
      padding: 10px 20px;
      cursor: pointer;
      outline: 0;
      transition: all 0.5s;
      border-radius: 3px;

      &:hover {
         background-color: #5d5de6;
         color: #fff;
         box-shadow: 0px 0px 15px #000;
      }
   }

   .error-message {
      display: none;
      color: red;
      font-size: 12px;
      margin-top: 4px;
   }

   .visible {
      display: block;
   }
}
</style>