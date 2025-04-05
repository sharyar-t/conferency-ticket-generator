<script setup lang="ts"></script>

<template>
  <div class="relative w-screen h-screen overflow-hidden">
    <!-- Pattern сверху -->
    <div class="absolute inset-0 bg-[url('/images/pattern-lines.svg')] bg-repeat z-10"></div>

    <!-- Основной фон -->
    <div
      class="absolute inset-0 bg-[url('/images/background-desktop.png')] bg-cover bg-center z-0"
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
      class="absolute bottom-0 left-0 z-20"
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

      <div class="flex flex-col items-center text-center gap-5 mb-12">
        <h1 class="text-preset-1">Your Journey to Coding Conf 2025 Starts Here!</h1>
        <p class="text-preset-4">Secure your spot at next year's biggest coding conference.</p>
      </div>

      <!-- Форма -->
      <div class="flex flex-col gap-6 min-w-[460px]">
        <div class="flex flex-col gap-3 w-full">
          <p class="text-preset-5">Upload Avatar</p>
          <!-- Label для аватара -->
          <label
            for="avatar"
            class="flex flex-col items-center border border-dashed border-neutral-500 justify-center w-full cursor-pointer p-4 rounded-lg text-center"
          >
            <!-- Иконка аватара -->
            <span class="border border-neutral-700 rounded-xl p-[10px] mb-4">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="30"
                height="30"
                fill="none"
                viewBox="0 0 30 30"
                class="text-orange-500"
              >
                <path
                  fill="currentColor"
                  fill-rule="evenodd"
                  d="M21.894 11.252a.264.264 0 0 1-.229-.225c-.368-2.634-2.51-5.924-6.663-5.924-4.465 0-6.3 3.636-6.657 5.928a.264.264 0 0 1-.228.22c-2.95.362-4.945 2.622-4.945 5.729a5.802 5.802 0 0 0 3.423 5.277 6.274 6.274 0 0 0 2.305.468h2.528a.45.45 0 0 0 .45-.45c0-.267-.233-.472-.5-.484a3.077 3.077 0 0 1-2.049-.9 3.123 3.123 0 0 1 0-4.418l3.461-3.462c.147-.146.307-.277.479-.392.076-.05.158-.085.236-.129.1-.054.196-.114.301-.158.1-.04.206-.065.308-.096.092-.027.181-.062.276-.081.191-.039.384-.056.578-.059.011 0 .022-.004.034-.004.01 0 .018.003.027.004.196.002.391.02.584.059.094.019.18.053.271.08.105.031.211.055.313.098.1.042.193.098.288.15.084.046.17.083.25.137.154.103.295.221.428.349.016.014.034.024.049.039l3.463 3.463a3.124 3.124 0 0 1 0 4.42c-.558.56-1.284.86-2.05.897-.266.013-.497.219-.497.486 0 .249.202.451.451.451h2.512c.435 0 1.314-.06 2.344-.473a5.794 5.794 0 0 0 3.394-5.272c0-3.104-1.991-5.363-4.935-5.728Z"
                  clip-rule="evenodd"
                />
                <path
                  fill="currentColor"
                  fill-rule="evenodd"
                  d="M18.464 19.62a.936.936 0 0 0 .663-1.6l-3.464-3.464a.938.938 0 0 0-.664-.275l-.014.002a.932.932 0 0 0-.65.274l-3.462 3.462a.936.936 0 1 0 1.326 1.325l1.864-1.862v6.479a.937.937 0 1 0 1.875 0v-6.48l1.864 1.863a.93.93 0 0 0 .662.275Z"
                  clip-rule="evenodd"
                />
              </svg>
            </span>

            <p class="text-preset-6 text-neutral-300">Drag and drop or click to upload</p>
          </label>

          <!-- Скрытый input для файлов -->
          <input id="avatar" type="file" class="hidden" accept="image/*" />

          <span class="text-neutral-300 flex items-center gap-2">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              fill="none"
              viewBox="0 0 16 16"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M2 8a6 6 0 1 0 12 0A6 6 0 0 0 2 8Z"
              />
              <path fill="currentColor" d="M8.004 10.462V7.596ZM8 5.57v-.042Z" />
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M8.004 10.462V7.596M8 5.569v-.042"
              />
            </svg>
            <span class="text-preset-7">Upload your photo (JPG or PNG, max size: 500KB).</span>
          </span>
        </div>
        <div class="flex flex-col gap-3">
          <label for="fullName" class="text-preset-5">Full Name</label>
          <input id="fullName" type="text" />
        </div>
        <div class="flex flex-col gap-3">
          <label for="email" class="text-preset-5">Email Address</label>
          <input id="email" type="email" placeholder="example@email.com" />
        </div>
        <div class="flex flex-col gap-3">
          <label for="githubUsername" class="text-preset-5">Email Address</label>
          <input id="githubUsername" type="text" placeholder="@yourusername" />
        </div>
        <div>
          <button class="button">Generate My Ticket</button>
        </div>
      </div>
    </main>
  </div>
</template>
