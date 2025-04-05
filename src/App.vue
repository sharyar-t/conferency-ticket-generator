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
    <div
      class="relative z-30 flex items-center justify-center h-full text-white text-3xl font-bold"
    >
      <!-- Лого -->
      <div class="flex items-center gap-4">
        <img src="/images/logo-mark.svg" alt="logo" />
        <span>Coding Conf</span>
      </div>
    </div>
  </div>
</template>
