<template>
  <UserFilter @filterChanged="updateFilter" />
  <div class="wrapper">
    <ul>
      <li v-for="user in filteredUsers" :key="user.id">
        <div class="user-card">
          <div class="icons">
            <FontAwesomeIcon
              class="icon"
              :icon="editIcon"
              @click="viewDetails(user)"
            />
            <FontAwesomeIcon
              class="icon"
              :icon="deleteIcon"
              @click="deleteUserHandler(user.id)"
            />
          </div>
          <div class="image-wrapper">
            <img :src="user.avatar" :alt="user.first_name" class="avatar" />
          </div>
          <div class="user-info">
            <h2 @click="viewDetails(user)">{{ user.first_name }}</h2>
            <p>
              <b>Email:</b><br />
              {{ user.email }}
            </p>
          </div>
        </div>
      </li>
    </ul>
    <UserDetails
      :users="users"
      :selectedUser="selectedUser"
      @closeModal="closeModal"
      :putUser="putUser"
    />
  </div>
</template>

<script>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { faEdit, faTrash } from "@fortawesome/free-solid-svg-icons";

import UserDetails from "./UserDetails.vue";
import UserFilter from "./UserFilter.vue";

export default {
  data() {
    return {
      editIcon: faEdit,
      deleteIcon: faTrash,
      selectedUser: null,
      filter: "",
    };
  },
  computed: {
    filteredUsers() {
      const normalizedSearchTerm = this.filter.toLowerCase();
      return this.users.filter((user) => {
        return (
          user.first_name.toLowerCase().includes(normalizedSearchTerm) ||
          user.email.toLowerCase().includes(normalizedSearchTerm)
        );
      });
    },
  },

  components: {
    FontAwesomeIcon,
    UserDetails,
    UserFilter,
  },
  props: {
    users: {
      type: Array,
      required: true,
    },
    deleteUser: {
      type: Function,
      required: true,
    },
    putUser: {
      type: Function,
      required: true,
    },
  },
  methods: {
    updateFilter(newFilter) {
      this.filter = newFilter;
    },
    deleteUserHandler(userId) {
      this.deleteUser(userId);
    },
    viewDetails(user) {
      this.selectedUser = user;
      document.body.classList.add("body-lock");
    },
    closeModal() {
      this.selectedUser = null;
    },
  },
};
</script>

<style scoped>
h2 {
  padding-top: 10px;
  font-size: 1.3rem;
  color: #4b4b4b;
  cursor: pointer;
}

h2:hover {
  color: #1497b8;
}

p {
  font-size: 14px;
  text-align: center;
  padding-top: 15px;
  line-height: 150%;
  color: #4b4b4b;
}

ul {
  list-style-type: none;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;
}
.wrapper {
  margin-top: 30px;
}
.user-card {
  position: relative;
  height: 120px;
  width: 400px;
  margin: 50px auto;
  background-color: #fff;
  border-radius: 10px;
  -webkit-box-shadow: 10px 10px 93px 0px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: 10px 10px 93px 0px rgba(0, 0, 0, 0.75);
  box-shadow: 10px 10px 93px 0px rgba(0, 0, 0, 0.75);
}

.icons {
  position: relative;
  top: 10px;
  left: 10px;
  width: 70px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-left: 300px;
}

.icon:hover {
  color: #1497b8;
  cursor: pointer;
}

.image-wrapper {
  border-radius: 10px;
  float: left;
  position: relative;
  left: 15px;
  top: -30px;
  height: 80px;
  width: 137px;
  -webkit-box-shadow: 10px 10px 60px 0px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: 10px 10px 60px 0px rgba(0, 0, 0, 0.75);
  box-shadow: 10px 10px 60px 0px rgba(0, 0, 0, 0.75);
  overflow: hidden;
}

.avatar {
  position: absolute;
  left: 50%;
  top: 50%;
  height: auto;
  width: 100%;
  -webkit-transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
}
</style>
