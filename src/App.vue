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

      <div class="flex flex-col items-center text-center gap-5">
        <h1 class="text-preset-1">Your Journey to Coding Conf 2025 Starts Here!</h1>
        <p class="text-preset-4">Secure your spot at next year's biggest coding conference.</p>
      </div>
    </main>
  </div>
</template>
