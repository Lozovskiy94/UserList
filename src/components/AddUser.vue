<template>
  <div>
    <Button :btnMessage="buttonMessage" @click="toggleForm" />
    <form @submit.prevent="newUser" v-show="showForm">
      <label class="text-field__label" for="name">Имя:</label>
      <input
        type="text"
        id="name"
        v-model="user.first_name"
        :class="{ error: !isNameValid }"
      />
      <div v-if="!isNameValid" class="error-message">
        Имя не может быть пустым
      </div>
      <label class="text-field__label" for="email">Email:</label>
      <input
        type="text"
        id="email"
        v-model="user.email"
        :class="{ error: !isEmailValid }"
      />
      <div v-if="!isEmailValid" class="error-message">
        Введите корректный email
      </div>

      <Button
        :disabled="!isFormValid"
        btnMessage="Добавить"
        @click="toggleForm"
      />
    </form>
  </div>
</template>

<script>
import Button from "./Button.vue";

export default {
  components: {
    Button,
  },
  data() {
    return {
      showForm: false,
      buttonMessage: "Добавить пользователя",
      user: {
        first_name: "",
        email: "",
      },
      isNameValid: true,
      isEmailValid: true,
    };
  },
  computed: {
    isFormValid() {
      return (
        this.isNameValid &&
        this.isEmailValid &&
        this.user.first_name !== "" &&
        this.user.email !== ""
      );
    },
  },
  methods: {
    toggleForm() {
      this.showForm = !this.showForm;
      if (this.showForm === true) {
        this.isNameValid = true;
        this.isEmailValid = true;
        this.user = {
          first_name: "",
          email: "",
        };
        this.buttonMessage = "Закрыть";
      } else {
        this.buttonMessage = "Добавить пользователя";
      }
    },
    newUser() {
      if (this.isFormValid) {
        this.buttonMessage = "Добавить пользователя";
        this.addUser(this.user);
        this.showForm = false;
        this.isNameValid = true;
        this.isEmailValid = true;
        this.user = {
          first_name: "",
          email: "",
        };
      }
    },
  },
  props: {
    addUser: {
      type: Function,
      required: true,
    },
  },
  watch: {
    "user.first_name"(newName) {
      this.isNameValid = newName.trim() !== "";
    },
    "user.email"(newEmail) {
      this.isEmailValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(newEmail);
    },
  },
};
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.text-field__label {
  display: block;
  margin-bottom: 0.25rem;
}

.error {
  border-color: red;
}

.error-message {
  color: red;
}
</style>
