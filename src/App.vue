<script>
import axios from "axios";

import UserCard from "./components/UserCard.vue";
import AddUser from "./components/AddUser.vue";

export default {
  data() {
    return {
      users: [],
      newUser: "",
      errors: "",
      selectedUser: null,
    };
  },
  mounted() {
    this.loadUsers();
  },
  methods: {
    async loadUsers() {
      try {
        const response = await axios.get("https://reqres.in/api/users");
        this.users = response.data.data;
      } catch (error) {
        console.error("Ошибка при загрузке пользователей", error);
      }
    },
    async addUser(newUser) {
      try {
        const response = await axios.post(
          "https://reqres.in/api/users",
          newUser
        );
        this.users.push(response.data);
      } catch (error) {
        console.error("Ошибка при добавлении пользователя:", error);
      }
    },
    async deleteUser(id) {
      try {
        const response = await axios.delete(
          `https://reqres.in/api/users/${id}`
        );

        const userIndex = this.users.findIndex((user) => user.id === id);
        if (userIndex !== -1) {
          this.users.splice(userIndex, 1);
        } else {
          console.error("Пользователь с указанным ID не найден.");
        }
      } catch (error) {
        console.error("Ошибка при удалении пользователя:", error);
      }
    },
    async putUser(updatedUser) {
      try {
        const response = await axios.put(
          `https://reqres.in/api/users/${updatedUser.id}`,
          updatedUser
        );
        const userIndex = this.users.findIndex(
          (user) => user.id === updatedUser.id
        );
        if (userIndex !== -1) {
          this.users.splice(userIndex, 1, updatedUser);
        } else {
          console.error("Пользователь с указанным ID не найден.");
        }
      } catch (error) {
        console.error("Ошибка при обновлении пользователя:", error);
      }
    },
  },
  components: {
    UserCard,
    AddUser,
  },
};
</script>

<template>
  <h1>Список пользователей</h1>
  <AddUser :addUser="addUser" />
  <UserCard :users="users" :deleteUser="deleteUser" :putUser="putUser" />
</template>

<style scoped></style>
