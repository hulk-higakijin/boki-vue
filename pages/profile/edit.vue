<template>
  <div class="w-screen h-screen text-gray-700">
    <Nav />

    <div class="bg-grey-lighter flex flex-col">
      <div class="container max-w-sm mx-auto flex-1 flex flex-col mt-20 px-2">
        <form @submit.prevent="updateProfile" class="bg-white px-6 py-8 rounded shadow-md text-black w-full">
          <h1 class="mb-8 text-3xl text-center">ユーザー情報を編集</h1>
          <img v-if="image_url" class="h-24 w-24 rounded-full mx-auto mb-4" :src="image_url" />
          <div v-else class="w-20 h-20 mx-auto">
            <SvgNoimage />
          </div>
          <div class="flex items-center justify-center w-full my-5">
            <label class="flex flex-col w-full h-20 border-4 border-gray-200 border-dashed hover:bg-gray-100 hover:border-gray-300 cursor-pointer">
              <div class="flex flex-col items-center justify-center pt-3">
                <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8 text-gray-400 group-hover:text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12" />
                </svg>
                <p class="pt-1 text-sm tracking-wider text-gray-400 group-hover:text-gray-600">アイコンをアップロード</p>
              </div>
              <input ref="avatar_preview" @change="uploadAvatar" type="file" accept="image/jpeg,image/png" class="opacity-0" />
            </label>
          </div>
          <input v-model="name" type="text" class="block border border-grey-light w-full p-3 rounded mb-4" name="name" placeholder="Name" />
          <div class="flex gap-x-5">
            <NuxtLink to="/profile" class="w-1/2 text-center py-3 rounded border border-indigo-500 text-indigo-500 my-1 hover:bg-gray-100">キャンセル</NuxtLink>
            <button type="submit" class="w-1/2 text-center py-3 rounded bg-indigo-500 text-white hover:bg-indigo-600 focus:outline-none my-1">更新</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  middleware({ redirect, store }) {
    if (!store.$auth.$state.loggedIn) {
      redirect('/')
    }
  },
  data() {
    return {
      name: this.$auth.user.name,
      // email: this.$auth.user.email,
      avatar: '',
      image_url: this.$auth.user.avatar_url,
    }
  },
  methods: {
    async updateProfile() {
      const config = {
        headers: {
          'content-type': 'multipart/form-data',
        },
      }
      const formData = new FormData()
      formData.append('user[name]', this.name)
      if (this.avatar) {
        formData.append('user[avatar]', this.avatar)
      }
      await this.$axios.$put(`/users/${this.$auth.user.id}`, formData, config).then((res) => {
        window.location.href = window.location.href.replace('/edit', '')
      })
    },

    uploadAvatar(e) {
      this.avatar = ''
      e.preventDefault()
      this.avatar = e.target.files[0]
      const file = this.$refs.avatar_preview.files[0]
      if (file) {
        this.image_url = URL.createObjectURL(file)
      }
    },
  },
}
</script>
