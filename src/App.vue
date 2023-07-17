<script setup>
  import LoginForm from './components/LoginForm.vue';
  import Image from './components/Image.vue';
</script>

<template>
    <div class="grid grid-cols-12">
      <div class="uno col-span-5">
        <LoginForm/>
      </div>
      <div class="dos col-span-7">
        <Image/>
      </div>
    </div>
</template>



<style scoped>
  .uno{
    background-color: #681400;
  }
</style>

