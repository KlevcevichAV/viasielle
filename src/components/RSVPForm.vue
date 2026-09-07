<template>
  <section class="rsvp">
    <div class="container">
      <h2 class="section-title">RSVP</h2>
      <p class="description">
        Пожалуйста, подтвердите ваше присутствие до 14.08 включительно, заполнив форму ниже.
      </p>

      <v-form ref="formRef" v-model="isFormValid" @submit.prevent="submitForm" class="rsvp-form">
        <!-- Attendance Question -->
        <div class="form-section">
          <p class="question-text">Разделите ли вы с нами радость этого дня?</p>
          <v-radio-group
            v-model="form.status"
            :rules="[rules.required]"
            required
            class="mt-2"
            @update:model-value="onStatusChange"
          >
            <v-radio label="С удовольствием приду" value="yes"></v-radio>
            <v-radio label="К сожалению, не смогу" value="no"></v-radio>
          </v-radio-group>
        </div>

        <!-- Declining Guest Name -->
        <div v-if="form.status === 'no'" class="form-section">
          <p class="question-text">Пожалуйста, представьтесь:</p>
          <v-text-field
            v-model="form.declinerName"
            label="Ваши Имя и Фамилия"
            :rules="[rules.required]"
            variant="outlined"
            density="comfortable"
            class="mt-2"
          ></v-text-field>
        </div>

        <template v-if="form.status === 'yes'">
          <!-- Guests Section -->
          <div class="form-section">
            <p class="question-text">Кто будет присутствовать?</p>
            <p class="sub-description">Пожалуйста, укажите имена и фамилии всех гостей, включая вашего +1, точно так же, как в приглашении.</p>
            
            <div v-for="(guest, index) in form.guests" :key="index" class="guest-input-wrapper">
              <v-text-field
                v-model="form.guests[index]"
                :label="index === 0 ? 'Ваши Имя и Фамилия' : `Имя гостя ${index + 1}`"
                :rules="[rules.required]"
                variant="outlined"
                density="comfortable"
                class="mb-2"
              ></v-text-field>
              <v-btn
                v-if="form.guests.length > 1"
                icon="mdi-delete-outline"
                variant="text"
                color="error"
                @click="removeGuest(index)"
                class="remove-guest-btn"
              ></v-btn>
            </div>
            
            <v-btn
              variant="outlined"
              prepend-icon="mdi-plus"
              @click="addGuest"
              class="mt-2"
            >
              Добавить гостя
            </v-btn>
          </div>

          <!-- Alcohol Preferences -->
          <div class="form-section">
            <p class="question-text">Пожелания по напиткам:</p>
            <v-select
              v-model="form.alcohol"
              :items="alcoholOptions"
              label="Выберите ваши предпочтения"
              multiple
              chips
              :rules="[rules.requiredSelection]"
              variant="outlined"
              density="comfortable"
              class="mt-2"
            ></v-select>
            
            <v-text-field
              v-if="form.alcohol.includes('Другое')"
              v-model="form.alcoholCustom"
              label="Что именно вы предпочитаете?"
              :rules="[rules.required]"
              variant="outlined"
              density="comfortable"
              class="mt-2"
            ></v-text-field>
          </div>

          <!-- Favorite Song -->
          <div class="form-section">
            <p class="question-text">Есть ли у вас любимая песня или исполнитель, которые точно заставят вас выйти на танцпол?</p>
            <v-text-field
              v-model="form.favoriteSong"
              label="Название трека"
              variant="outlined"
              density="comfortable"
              class="mt-2"
            ></v-text-field>
          </div>

          <!-- Fun Fact -->
          <div class="form-section">
            <p class="question-text">Расскажите забавный или интересный факт о себе (или воспоминание, связанное с женихом/невестой)</p>
            <v-textarea
              v-model="form.funFact"
              label="По желанию"
              variant="outlined"
              auto-grow
              rows="2"
              class="mt-2"
            ></v-textarea>
          </div>

          <!-- Wishes / Notes -->
          <div class="form-section">
            <p class="question-text">Ваши пожелания или особые пометки для нас:</p>
            <v-textarea
              v-model="form.wishes"
              label="Ваше сообщение"
              variant="outlined"
              auto-grow
              rows="3"
              class="mt-2"
            ></v-textarea>
          </div>
        </template>

        <!-- Submit Button -->
        <div class="form-actions mt-6">
          <v-btn
            type="submit"
            color="var(--color-primary)"
            size="large"
            block
            :loading="loading"
            :disabled="!isFormValid || loading"
            class="submit-btn"
          >
            {{ form.status === 'no' ? 'Отклонить приглашение' : 'Отправить анкету' }}
          </v-btn>
        </div>
      </v-form>
    </div>
  </section>
</template>

<script setup>
import { ref, reactive } from 'vue'

const formRef = ref(null)
const isFormValid = ref(false)
const loading = ref(false)

const alcoholOptions = [
  'Вино',
  'Шампанское',
  'Виски',
  'Пиво',
  'Другое'
]

const form = reactive({
  status: 'yes',
  guests: [''],
  declinerName: '',
  favoriteSong: '',
  wishes: '',
  alcohol: [],
  alcoholCustom: '',
  funFact: ''
})

const rules = {
  required: v => !!v || 'Это поле обязательно',
  requiredSelection: v => (v && v.length > 0) || 'Пожалуйста, выберите хотя бы один вариант'
}

const onStatusChange = () => {
  if (form.status === 'no') {
    form.guests = ['']
  } else {
    form.declinerName = ''
  }
}

const addGuest = () => {
  form.guests.push('')
}

const removeGuest = (index) => {
  form.guests.splice(index, 1)
}

const submitForm = async () => {
  const { valid } = await formRef.value.validate()
  if (!valid) return

  loading.value = true

  const token = import.meta.env.VITE_TELEGRAM_TOKEN
  const chatId = import.meta.env.VITE_CHAT_ID

  let message = `🔔 **Новый ответ на приглашение!**\n`
  message += `━━━━━━━━━━━━━━━━━━\n`
  
  if (form.status === 'no') {
    message += `📍 **Статус:** ❌ К сожалению, не смогу\n`
    message += `👤 **От:** ${form.declinerName || 'Неизвестно'}\n`
  } else {
    message += `📍 **Статус:** ✅ С удовольствием приду\n`
    message += `👥 **Гости:**\n${form.guests.filter(g => g).map(g => `  • ${g}`).join('\n')}\n`
    
    const alcoholList = form.alcohol.map(item => {
      if (item === 'Другое' && form.alcoholCustom) {
        return `Другое (${form.alcoholCustom})`
      }
      return item
    })
    message += `🍷 **Алкоголь:** ${alcoholList.join(', ')}\n`
    
    if (form.favoriteSong) {
      message += `🎵 **Любимая песня:** ${form.favoriteSong}\n`
    }
    
    if (form.funFact) {
      message += `✨ **Интересный факт:** ${form.funFact}\n`
    }
    
    if (form.wishes) {
      message += `💬 **Пожелания:** ${form.wishes}\n`
    }
  }
  
  message += `━━━━━━━━━━━━━━━━━━`

  try {
    const response = await fetch(`https://api.telegram.org/bot${token}/sendMessage`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        chat_id: chatId,
        text: message,
        parse_mode: 'Markdown'
      })
    })

    if (response.ok) {
      alert('Спасибо! Ваш ответ успешно отправлен.')
      resetForm()
    } else {
      throw new Error('Failed to send message')
    }
  } catch (error) {
    console.error('Error:', error)
    alert('Произошла ошибка при отправке. Пожалуйста, попробуйте еще раз.')
  } finally {
    loading.value = false
  }
}

const resetForm = () => {
  form.status = 'yes'
  form.guests = ['']
  form.declinerName = ''
  form.favoriteSong = ''
  form.wishes = ''
  form.alcohol = []
  form.alcoholCustom = ''
  form.funFact = ''
  if (formRef.value) {
    formRef.value.resetValidation()
  }
}
</script>

<style scoped>
.rsvp {
  padding: 4rem 1rem;
  background-color: var(--color-background-soft);
  color: var(--color-text);
}

.container {
  max-width: 600px;
  margin: 0 auto;
}

.section-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 2.5rem;
  text-align: center;
  margin-bottom: 1rem;
}

.description {
  font-size: 1.1rem;
  text-align: center;
  margin-bottom: 3rem;
  line-height: 1.6;
  opacity: 0.9;
}

.form-section {
  margin-bottom: 2rem;
}

.question-text {
  font-weight: 600;
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
  color: var(--color-heading);
}

.sub-description {
  font-size: 0.9rem;
  margin-bottom: 1rem;
  opacity: 0.7;
}

.guest-input-wrapper {
  display: flex;
  align-items: flex-start;
  gap: 0.5rem;
}

.remove-guest-btn {
  margin-top: 4px;
}

.submit-btn {
  color: white !important;
  font-weight: 600;
  letter-spacing: 1px;
}

:deep(.v-label) {
  color: var(--color-text) !important;
}

:deep(.v-field__outline) {
  color: var(--color-border) !important;
}

:deep(.v-field--focused .v-field__outline) {
  color: var(--color-primary) !important;
}

@media (max-width: 600px) {
  .section-title {
    font-size: 2rem;
  }
  
  .rsvp {
    padding: 3rem 1rem;
  }
}
</style>