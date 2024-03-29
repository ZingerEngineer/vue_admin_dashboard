<template>
  <div
    class="flex min-h-full items-center justify-center py-12 px-4 sm:px-6 lg:px-8"
  >
    <div class="w-full max-w-md space-y-8">
      <div>
        <img
          class="mx-auto h-12 w-auto"
          src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=600"
          alt="Your Company"
        />
        <h2
          class="mt-6 text-center text-3xl font-bold tracking-tight text-gray-900"
        >
          Register your account.
        </h2>
      </div>
      <form
        class="mt-8 space-y-6"
        @submit.prevent
      >
        <input
          type="hidden"
          name="remember"
          value="true"
        />
        <div class="-space-y-px rounded-md shadow-sm">
          <div>
            <label
              for="email-address"
              class="sr-only"
              >Email address</label
            >
            <input
              v-model="emailText"
              @change="
                () => {
                  validateEmail(emailText), isValidSubmitValueChange()
                }
              "
              id="email-address"
              name="email"
              type="email"
              autocomplete="email"
              required=""
              class="relative block w-full appearance-none rounded-none rounded-t-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
              placeholder="Email address"
            />
          </div>
          <div>
            <label
              for="password"
              class="sr-only"
              >Password</label
            >
            <input
              v-model="passwordText"
              @change="
                () => {
                  validatePassword(passwordText), isValidSubmitValueChange()
                }
              "
              id="password"
              name="password"
              type="password"
              autocomplete="current-password"
              required=""
              class="relative block w-full appearance-none rounded-none rounded-b-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
              placeholder="Password"
            />
          </div>
        </div>
        <div class="flex items-center justify-between">
          <div class="flex items-center">
            <input
              id="remember-me"
              name="remember-me"
              type="checkbox"
              class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500"
            />
            <label
              for="remember-me"
              class="ml-2 block text-sm text-gray-900"
              >Remember me</label
            >
          </div>
        </div>

        <div>
          <button
            class="group relative flex w-full justify-center rounded-md border border-transparent bg-indigo-600 py-2 px-4 text-sm font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
            @click="register"
          >
            <span class="absolute inset-y-0 left-0 flex items-center pl-3">
              <LockClosedIcon
                v-if="!isValidSubmit"
                class="h-5 w-5 text-indigo-500 group-hover:text-indigo-400"
                aria-hidden="true"
              />
            </span>
            register
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
import { LockClosedIcon } from '@heroicons/vue/20/solid'
import { defineComponent } from '@vue/runtime-core'
import {
  emailInputValidation,
  passwordInputValidation
} from '../utils/inputValidation'
import axios from 'axios'
export default defineComponent({
  name: 'register-page',
  components: {
    LockClosedIcon
  },
  data() {
    return {
      backEndPostUrl: 'http://localhost:3001/api/users/signup',
      isValidEmailInput: false,
      isValidPasswordInput: false,
      isValidSubmit: false,
      emailText: '',
      passwordText: '',
      isAbleToSubmit: false
    }
  },
  methods: {
    validateEmail(email: string) {
      this.isValidEmailInput = emailInputValidation(email)
    },
    validatePassword(password: string) {
      this.isValidPasswordInput = passwordInputValidation(password)
    },
    isValidSubmitValueChange() {
      this.isValidSubmit = this.isValidEmailInput && this.isValidEmailInput
    },
    async register() {
      if (this.isValidSubmit) {
        try {
          await axios
            .post(this.backEndPostUrl, {
              email: this.emailText,
              password: this.passwordText
            })
            .then((res) => {
              localStorage.setItem('token', res.data.token)
            })
          window.location.replace('/cp/user_home')
        } catch (error) {
          console.log(error)
        }
      }
    }
  }
})
</script>
