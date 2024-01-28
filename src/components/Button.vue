<template>
  <button :disabled="isDisabled" class="btn">
    {{ btnMessage }}
  </button>
</template>

<script>
export default {
  props: {
    btnMessage: {
      type: String,
    },
    isDisabled: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style scoped>
.btn {
  margin-top: 20px;
  margin-bottom: 20px;
  display: inline-block;
  padding: 0.75rem 1.25rem;
  border: none;
  border-radius: 10rem;
  color: #fff;
  text-transform: uppercase;
  font-size: 1rem;
  letter-spacing: 0.15rem;
  transition: all 0.3s;
  position: relative;
  overflow: hidden;
  z-index: 1;
  cursor: pointer;
}

.btn:after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #0cf;
  border-radius: 10rem;
  z-index: -2;
}

.btn:before {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0%;
  height: 100%;
  background-color: #1497b8;
  transition: all 0.3s;
  border-radius: 10rem;
  z-index: -1;
}

.btn:hover {
  color: #fff;
}

.btn:hover:before {
  width: 100%;
}

.btn:disabled,
.btn:disabled:before,
.btn:disabled:after {
  background: #ccc;
  cursor: not-allowed;
}
</style>
