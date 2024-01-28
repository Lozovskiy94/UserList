<template>
  <div>
    <div v-if="selectedUser" class="modal" @click="closeModal">
      <div class="modal-content">
        <span class="close" @click="closeModal">&times;</span>
        <h2>{{ selectedUser.first_name }}</h2>
        <img
          :src="selectedUser.avatar"
          :alt="selectedUser.first_name"
          class="avatar"
        />
        <p>Email: {{ selectedUser.email }}</p>
        <br />
        <p>
          Номер телефона:
          <span v-if="!editingPhone">{{
            selectedUser.phone || "Не указан"
          }}</span>
          <input
            v-if="editingPhone"
            v-model="selectedUser.phone"
            placeholder="Введите номер телефона"
          />
          <br />
          <Button
            btnMessage="Редактировать"
            v-if="!editingPhone"
            @click="toggleEditing('phone')"
          />
        </p>
        <p>
          Адрес проживания:
          <span v-if="!editingAddress">{{
            selectedUser.address || "Не указан"
          }}</span>
          <input
            v-if="editingAddress"
            v-model="selectedUser.address"
            placeholder="Введите адрес"
          />
          <br />
          <Button
            btnMessage="Редактировать"
            v-if="!editingAddress"
            @click="toggleEditing('address')"
          />
        </p>
        <Button
          btnMessage="Сохранить изменения"
          v-if="editingAddress || editingPhone"
          @click="saveChanges"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Button from "./Button.vue";

export default {
  components: {
    Button,
  },
  props: {
    users: {
      type: Array,
      required: true,
    },
    selectedUser: {
      type: Object,
      default: null,
    },
    putUser: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      editingPhone: false,
      editingAddress: false,
    };
  },
  methods: {
    closeModal() {
      if (
        event.target.classList.contains("modal") ||
        event.target.classList.contains("close")
      ) {
        this.$emit("closeModal");
        this.editingPhone = false;
        this.editingAddress = false;
        document.body.classList.remove("body-lock");
      }
    },
    toggleEditing(field) {
      if (field === "phone") {
        this.editingPhone = !this.editingPhone;
      } else if (field === "address") {
        this.editingAddress = !this.editingAddress;
      }
    },
    saveChanges() {
      this.putUser(this.selectedUser);
      this.$emit("saveChanges", this.selectedUser);
      this.editingPhone = false;
      this.editingAddress = false;
    },
  },
};
</script>

<style scoped>
.modal {
  display: block;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 50%;
  z-index: 10;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

ul {
  list-style-type: none;
  cursor: pointer;
}

li {
  margin: 10px 0;
  padding: 5px;
  border: 1px solid #ccc;
}

.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  margin-top: 10px;
  margin-bottom: 20px;
}
</style>
