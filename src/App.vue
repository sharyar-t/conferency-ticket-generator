<script setup lang="ts">
import { ref } from 'vue'
import { useDropZone } from '@vueuse/core'
import { useForm } from 'vee-validate'
import { toTypedSchema } from '@vee-validate/zod'
import { z } from 'zod'
import IconInfo from '@/components/IconInfo.vue'
import IconGithub from '@/components/IconGithub.vue'
import IconUpload from '@/components/IconUpload.vue'

const schema = toTypedSchema(
  z.object({
    fullName: z.string({ required_error: 'Please enter your full name' }).nonempty(),
    email: z
      .string({ required_error: 'Please enter your email' })
      .nonempty()
      .email('Please enter a valid email'),
    githubUsername: z
      .string({ required_error: 'Please enter your GitHub username' })
      .nonempty()
      .startsWith('@', 'Github Username must start with @'),
  }),
)

const { errors, defineField, handleSubmit } = useForm({
  validationSchema: schema,
})

const dropZoneRef = ref<HTMLElement | null>(null)
const avatarPreview = ref<string | null>(null)
const avatarError = ref('')

const [fullName, fullNameAttrs] = defineField('fullName')
const [email, emailAttrs] = defineField('email', {
  validateOnModelUpdate: false,
})
const [githubUsername, githubUsernameAttrs] = defineField('githubUsername')

const isGenerated = ref(false)

function onDrop(files: File[] | null) {
  if (files) {
    previewAvatar(files[0])
  }
}

function triggerDropZoneRef() {
  dropZoneRef.value?.click()
}

function resetAvatarPreview() {
  avatarPreview.value = null
}

function handleAvatarChange(e: Event) {
  const input = e.currentTarget as HTMLInputElement | null
  const files = input?.files
  if (files && files.length > 0) {
    previewAvatar(files[0])
  }
}

function previewAvatar(file: File) {
  const reader = new FileReader()
  reader.onloadend = function () {
    avatarPreview.value = reader.result as string
  }
  reader.readAsDataURL(file)
}

const { isOverDropZone } = useDropZone(dropZoneRef, {
  onDrop,
  dataTypes: ['image/jpeg'],
  multiple: false,
})

const onSubmit = handleSubmit((values) => {
  if (!avatarPreview.value) {
    avatarError.value = 'Please upload an avatar'
    return
  }
  console.log(values)
  isGenerated.value = true
})
</script>

<template>
  <div class="relative h-screen w-screen overflow-hidden">
    <!-- Pattern сверху -->
    <div class="absolute inset-0 z-10 bg-[url('/images/pattern-lines.svg')] bg-repeat"></div>

    <!-- Основной фон -->
    <div
      class="absolute inset-0 z-0 hidden bg-[url('/images/background-desktop.png')] bg-cover bg-center lg:block"
    ></div>
    <div
      class="absolute inset-0 z-0 hidden bg-[url('/images/background-tablet.png')] bg-cover bg-center md:block lg:hidden"
    ></div>
    <div
      class="absolute inset-0 z-0 bg-[url('/images/background-mobile.png')] bg-cover bg-center md:hidden"
    ></div>

    <!-- SVG в правом верхнем углу -->
    <img
      src="/images/pattern-squiggly-line-top.svg"
      class="absolute top-0 right-0 z-20"
      alt="line top-right"
    />

    <!-- SVG в левом нижнем углу -->
    <img
      src="/images/pattern-squiggly-line-bottom-desktop.svg"
      class="absolute bottom-0 left-0 z-20 hidden md:block"
      alt="line bottom-left"
    />
    <img
      src="/images/pattern-squiggly-line-bottom-mobile-tablet.svg"
      class="absolute bottom-0 left-0 z-20 md:hidden"
      alt="line bottom-left"
    />

    <!-- Контент -->
    <main
      class="text-neutral-0 relative z-30 mx-auto flex h-full max-w-[890px] flex-col items-center justify-center"
    >
      <!-- Лого -->
      <div class="mb-14 flex items-center gap-4">
        <img src="/images/logo-mark.svg" alt="logo" />
        <span>Coding Conf</span>
      </div>
      <template v-if="isGenerated">
        <div class="mb-12 flex flex-col items-center gap-5 text-center">
          <h1 class="text-preset-1-mobile md:text-preset-1">
            Congrats,
            <span
              class="to-neutral-0 bg-gradient-to-r from-orange-500 bg-clip-text text-transparent"
              >{{ fullName }} </span
            >! <br />Your ticket is ready!
          </h1>
          <p class="text-preset-4-mobile md:text-preset-4">
            We've emailed your ticket to<br /><span class="text-orange-700">{{ email }}</span> and
            will send updates in the run up to the event.
          </p>
        </div>

        <div class="relative h-[160px] w-[343px] md:h-[280px] md:w-[600px]">
          <img
            src="/images/pattern-ticket.svg"
            class="absolute top-0 right-0 z-30"
            alt="line top-right"
          />
          <div class="relative z-40 flex h-full flex-col justify-between p-6">
            <div class="flex gap-5">
              <img src="/images/logo-mark.svg" alt="logo" />
              <div>
                <div class="text-preset-2-mobile md:text-preset-2">Coding Conf</div>
                <div class="text-preset-6 text-neutral-300">Jan 31, 2025 / Austin, TX</div>
              </div>
            </div>
            <div class="flex items-center gap-4">
              <img
                v-if="avatarPreview"
                :src="avatarPreview"
                class="h-[80px] w-[80px] rounded-xl object-cover"
                :alt="fullName"
              />
              <div class="flex flex-col gap-2">
                <div class="text-preset-3-mobile md:text-preset-3">{{ fullName }}</div>
                <div class="flex gap-2">
                  <IconGithub />
                  <span>{{ githubUsername }}</span>
                </div>
              </div>
              <div
                class="text-preset-3-mobile md:text-preset-3 absolute top-[50%] right-4 translate-y-[-50%] text-neutral-500 [writing-mode:vertical-lr] md:right-8"
              >
                #01609
              </div>
            </div>
          </div>
        </div>
      </template>
      <template v-else>
        <div class="mb-12 flex flex-col items-center gap-5 text-center">
          <h1 class="text-preset-1-mobile md:text-preset-1">
            Your Journey to Coding Conf 2025 Starts Here!
          </h1>
          <p class="text-preset-4-mobile md:text-preset-4">
            Secure your spot at next year's biggest coding conference.
          </p>
        </div>

        <!-- Форма -->
        <div class="flex w-full max-w-[460px] flex-col gap-6">
          <div class="flex w-full flex-col gap-3">
            <label class="text-preset-5" @click="triggerDropZoneRef">Upload Avatar</label>
            <!-- Label для аватара -->
            <label
              for="avatar"
              class="flex w-full cursor-pointer flex-col items-center justify-center rounded-lg border border-dashed border-neutral-500 p-4 text-center"
              ref="dropZoneRef"
            >
              <!-- Иконка аватара -->
              <span class="mb-4 rounded-xl border border-neutral-700 p-[10px]">
                <img
                  v-if="avatarPreview"
                  :src="avatarPreview"
                  class="h-[30px] w-[30px] object-cover"
                  alt="Your avatar"
                />
                <IconUpload v-else />
              </span>
              <p v-if="isOverDropZone" class="text-preset-6 text-neutral-300">Drop it here</p>
              <div v-else-if="avatarPreview" class="flex items-center gap-2">
                <button
                  class="text-preset-7 rounded-sm bg-neutral-700 px-2 py-1"
                  @click="resetAvatarPreview"
                >
                  Remove image
                </button>
                <button
                  class="text-preset-7 rounded-sm bg-neutral-700 px-2 py-1"
                  @click="triggerDropZoneRef"
                >
                  Change image
                </button>
              </div>
              <p v-else class="text-preset-6 text-neutral-300">Drag and drop or click to upload</p>
            </label>

            <!-- Скрытый input для файлов -->
            <input
              id="avatar"
              type="file"
              class="hidden"
              accept="image/*"
              @change="handleAvatarChange"
            />

            <span v-if="avatarError" class="text-preset-7 flex items-center gap-2 text-orange-500">
              <IconInfo />
              <span>{{ avatarError }}</span>
            </span>
            <span v-else class="text-preset-7 flex items-center gap-2 text-neutral-300">
              <IconInfo />
              <span>Upload your photo (JPG or PNG, max size: 500KB).</span>
            </span>
          </div>
          <div class="flex flex-col gap-3">
            <label for="fullName" class="text-preset-5">Full Name</label>
            <input v-model="fullName" v-bind="fullNameAttrs" id="fullName" type="text" />
            <p v-if="errors.fullName" class="text-preset-7 flex items-center gap-2 text-orange-500">
              <IconInfo />
              {{ errors.fullName }}
            </p>
          </div>
          <div class="flex flex-col gap-3">
            <label for="email" class="text-preset-5">Email Address</label>
            <input
              v-model="email"
              v-bind="emailAttrs"
              id="email"
              type="email"
              placeholder="example@email.com"
            />
            <p v-if="errors.email" class="text-preset-7 flex items-center gap-2 text-orange-500">
              <IconInfo />
              {{ errors.email }}
            </p>
          </div>
          <div class="flex flex-col gap-3">
            <label for="githubUsername" class="text-preset-5">GitHub Username</label>
            <input
              v-model="githubUsername"
              v-bind="githubUsernameAttrs"
              id="githubUsername"
              type="text"
              placeholder="@yourusername"
            />
            <p
              v-if="errors.githubUsername"
              class="text-preset-7 flex items-center gap-2 text-orange-500"
            >
              <IconInfo />
              {{ errors.githubUsername }}
            </p>
          </div>
          <div>
            <button class="button" @click="onSubmit">Generate My Ticket</button>
          </div>
        </div>
      </template>
    </main>
  </div>
</template>
