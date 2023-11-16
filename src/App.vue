<template>
  <div>
    <RegistrationForm :default-values="{
      username: 'default username',
      email: 'default email',
      password: 'default password',
      password_repeat: 'default password_repeat',
    }" />
  </div>
</template>
  
<script>
import RegistrationForm from '@/components/RegistrationForm.vue';

export default {
  components: {
    RegistrationForm
  }
};
</script>
  
<style></style>