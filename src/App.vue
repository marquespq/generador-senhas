<template>
  <div id="app" >
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <password-generator></password-generator>
  </div>
</template>

<script>
import PasswordGenerator from './components/PasswordGenerator.vue';
import './styles.css';

export default {
  name: 'App',
  components: {
    PasswordGenerator
  }
};
</script>
