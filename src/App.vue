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
  <div class="relative w-screen h-screen overflow-hidden">
    <!-- Pattern сверху -->
    <div class="absolute inset-0 bg-[url('/images/pattern-lines.svg')] bg-repeat z-10"></div>

    <!-- Основной фон -->
    <div
      class="hidden lg:block absolute inset-0 bg-[url('/images/background-desktop.png')] bg-cover bg-center z-0"
    ></div>
    <div
      class="hidden md:block lg:hidden absolute inset-0 bg-[url('/images/background-tablet.png')] bg-cover bg-center z-0"
    ></div>
    <div
      class="md:hidden absolute inset-0 bg-[url('/images/background-mobile.png')] bg-cover bg-center z-0"
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
      class="hidden md:block absolute bottom-0 left-0 z-20"
      alt="line bottom-left"
    />
    <img
      src="/images/pattern-squiggly-line-bottom-mobile-tablet.svg"
      class="md:hidden absolute bottom-0 left-0 z-20"
      alt="line bottom-left"
    />

    <!-- Контент -->
    <main
      class="max-w-[890px] mx-auto relative z-30 flex flex-col items-center justify-center h-full text-neutral-0"
    >
      <!-- Лого -->
      <div class="flex items-center gap-4 mb-14">
        <img src="/images/logo-mark.svg" alt="logo" />
        <span>Coding Conf</span>
      </div>
      <template v-if="isGenerated">
        <div class="flex flex-col items-center text-center gap-5 mb-12">
          <h1 class="text-preset-1-mobile md:text-preset-1">
            Congrats,
            <span
              class="text-transparent bg-clip-text bg-gradient-to-r from-orange-500 to-neutral-0"
              >{{ fullName }} </span
            >! <br />Your ticket is ready!
          </h1>
          <p class="text-preset-4-mobile md:text-preset-4">
            We've emailed your ticket to<br /><span class="text-orange-700">{{ email }}</span> and
            will send updates in the run up to the event.
          </p>
        </div>

        <div class="relative w-[343px] h-[160px] md:w-[600px] md:h-[280px]">
          <img
            src="/images/pattern-ticket.svg"
            class="absolute top-0 right-0 z-30"
            alt="line top-right"
          />
          <div class="z-40 p-6 flex flex-col justify-between h-full relative">
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
                class="w-[80px] h-[80px] object-cover rounded-xl"
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
                class="absolute top-[50%] right-4 md:right-8 translate-y-[-50%] text-neutral-500 text-preset-3-mobile md:text-preset-3 [writing-mode:vertical-lr]"
              >
                #01609
              </div>
            </div>
          </div>
        </div>
      </template>
      <template v-else>
        <div class="flex flex-col items-center text-center gap-5 mb-12">
          <h1 class="text-preset-1-mobile md:text-preset-1">
            Your Journey to Coding Conf 2025 Starts Here!
          </h1>
          <p class="text-preset-4-mobile md:text-preset-4">
            Secure your spot at next year's biggest coding conference.
          </p>
        </div>

        <!-- Форма -->
        <div class="flex flex-col gap-6 max-w-[460px] w-full">
          <div class="flex flex-col gap-3 w-full">
            <label class="text-preset-5" @click="triggerDropZoneRef">Upload Avatar</label>
            <!-- Label для аватара -->
            <label
              for="avatar"
              class="flex flex-col items-center border border-dashed border-neutral-500 justify-center w-full cursor-pointer p-4 rounded-lg text-center"
              ref="dropZoneRef"
            >
              <!-- Иконка аватара -->
              <span class="border border-neutral-700 rounded-xl p-[10px] mb-4">
                <img
                  v-if="avatarPreview"
                  :src="avatarPreview"
                  class="w-[30px] h-[30px] object-cover"
                  alt="Your avatar"
                />
                <IconUpload v-else />
              </span>
              <p v-if="isOverDropZone" class="text-preset-6 text-neutral-300">Drop it here</p>
              <div v-else-if="avatarPreview" class="flex items-center gap-2">
                <button
                  class="text-preset-7 bg-neutral-700 px-2 py-1 rounded-sm"
                  @click="resetAvatarPreview"
                >
                  Remove image
                </button>
                <button
                  class="text-preset-7 bg-neutral-700 px-2 py-1 rounded-sm"
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

            <span v-if="avatarError" class="flex items-center gap-2 text-preset-7 text-orange-500">
              <IconInfo />
              <span>{{ avatarError }}</span>
            </span>
            <span v-else class="text-neutral-300 flex items-center gap-2 text-preset-7">
              <IconInfo />
              <span>Upload your photo (JPG or PNG, max size: 500KB).</span>
            </span>
          </div>
          <div class="flex flex-col gap-3">
            <label for="fullName" class="text-preset-5">Full Name</label>
            <input v-model="fullName" v-bind="fullNameAttrs" id="fullName" type="text" />
            <p v-if="errors.fullName" class="flex items-center gap-2 text-preset-7 text-orange-500">
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
            <p v-if="errors.email" class="flex items-center gap-2 text-preset-7 text-orange-500">
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
              class="flex items-center gap-2 text-preset-7 text-orange-500"
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
