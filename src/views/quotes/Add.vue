<template>
  <div class="add">
    <Layout>
      <div class="title">
        <h1>เพิ่มคำคม</h1>
      </div>
      <br />

      <form @submit.prevent="handleSubmit">
        <div>
          <label for="add-quote">คำคมที่ต้องการเพิ่ม</label>
          <br />
          <input
            type="text"
            autocomplete="off"
            id="add-quote"
            required
            :disabled="store.getters.isLoading"
            v-model="insertQuote.quote"
            placeholder="แกอ่ะ เข้ามานั่งในใจเราหน่อยจิ"
            maxlength="30"
          />
        </div>
        <br />
        <div class="submit disable-select">
          <button type="submit">เพิ่มคำคม</button>
        </div>
      </form>
    </Layout>
  </div>
</template>

<script lang="ts" setup>
import { reactive } from 'vue';
import { useStore } from 'vuex';
import { addQuote } from '@/backend/database';

import Layout from '@/components/Layout.component.vue'
import supabase from '@/backend/app';

const store = useStore()

const insertQuote = reactive({
  quote: '',
})

const handleSubmit = async () => {
  store.dispatch('setLoading', true)

  const user = supabase.auth.user()

  const { error } = await addQuote(insertQuote.quote, user!.id)
  if (error) throw Error(error)

  alert('เพิ่มคำคมเรียบร้อยแล้ว')

  insertQuote.quote = ''

  store.dispatch('setLoading', false)
}
</script>

<style scoped>
.add {
  @apply drop-shadow-lg;
}

.title h1 {
  @apply text-5xl lg:text-7xl font-bold;
}

form input {
  @apply text-black rounded px-2 py-1 w-auto;
}

form label {
  @apply text-base;
}

.submit {
  @apply flex justify-center items-center;
}

.submit button {
  @apply px-4 py-2 bg-green-500 rounded-full hover:bg-green-600 transition-all;
}
</style>