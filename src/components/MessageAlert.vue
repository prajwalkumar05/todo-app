<!-- Toast.vue -->
<template>
    <div v-if="visible" class="toast">
      {{ message }}
    </div>
  </template>
  
  <script>
  export default {
    props: {
      message: {
        type: String,
        default: ''
      }
    },
    data() {
      return {
        visible: false
      };
    },
    methods: {
      showToast() {
        this.visible = true;
        setTimeout(() => {
          this.visible = false;
        }, 1000);
      }
    }
  }
  </script>
  
  <style scoped>
  /* Styles for the toast */
  .toast {
    position: fixed;
    top: 10px;
    left: 50%;
    transform: translateX(-50%);
    background-color: #FF5631 ;
    color: #CEBEA4;
    padding: 1rem 3rem;
    border-radius: 5px;
    z-index: 999;
    font-size: 1.8rem;
  }
  </style>
  