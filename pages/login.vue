<template>
  <div class="h-screen overflow-hidden">
    <!-- Section 1 -->
    <Nav />

    <section class="flex flex-col md:flex-row h-full items-center max-w-7xl mx-auto">
      <div class="bg-indigo-600 hidden lg:block w-full md:w-1/2 xl:w-2/3 h-full">
        <img src="https://source.unsplash.com/random" alt="" class="w-full h-full object-cover" />
      </div>

      <div class="bg-white w-full md:max-w-md lg:max-w-full md:mx-auto md:mx-0 md:w-1/2 xl:w-1/3 h-full px-6 lg:px-16 xl:px-12 flex items-center justify-center">
        <div class="w-full h-100">
          <h1 class="text-xl md:text-2xl font-bold leading-tight mt-12">Log in to your account</h1>

          <form @submit.prevent="login" class="mt-6">
            <div>
              <label class="block text-gray-700">Email Address</label>
              <input v-model="email" type="email" name="" id="" placeholder="Enter Email Address" class="w-full px-4 py-3 rounded-lg bg-gray-200 mt-2 border focus:border-blue-500 focus:bg-white focus:outline-none" autofocus required />
            </div>

            <div class="mt-4">
              <label class="block text-gray-700">Password</label>
              <input v-model="password" type="password" name="" id="" placeholder="Enter Password" minlength="6" class="w-full px-4 py-3 rounded-lg bg-gray-200 mt-2 border focus:border-blue-500 focus:bg-white focus:outline-none" required />
            </div>

            <!-- <div class="text-right mt-2">
              <a href="#" class="text-sm font-semibold text-gray-700 hover:text-blue-700 focus:text-blue-700">Forgot Password?</a>
            </div> -->

            <ul v-show="errors[0]" class="mt-4 pl-5">
              <li v-for="error in errors" :key="error.id" class="text-sm text-red-500 list-disc">{{ error }}</li>
            </ul>

            <button type="submit" class="w-full block bg-indigo-500 hover:bg-indigo-400 focus:bg-indigo-400 text-white font-semibold rounded-lg px-4 py-3 mt-6">Log In</button>
          </form>

          <!-- <hr class="my-6 border-gray-300 w-full" /> -->

          <!-- <button type="button" class="w-full block bg-white hover:bg-gray-100 focus:bg-gray-100 text-gray-900 font-semibold rounded-lg px-4 py-3 border border-gray-300">
            <div class="flex items-center justify-center">
              <SvgGoogle />
              <span class="ml-4"> Log in with Google</span>
            </div>
          </button> -->

          <p class="mt-8">Need an account? <NuxtLink to="/signup" class="text-blue-500 hover:text-blue-700 font-semibold">Create an account</NuxtLink></p>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  middleware({ redirect, store }) {
    if (store.$auth.state.loggedIn) {
      redirect('/dashbord')
    }
  },
  data() {
    return {
      email: '',
      password: '',
      errors: [],
    }
  },
  methods: {
    async login() {
      await this.$auth
        .loginWith('local', {
          data: {
            email: this.email,
            password: this.password,
          },
        })
        .then((res) => {
          console.log(res)
          this.$router.push('/dashbord')
        })
        .catch((e) => {
          this.errors = e.response.data.errors
        })
    },
  },
}
</script>
