<template>
  <label
    :for="id + '_button'"
    :class="{ active: isActive }"
    class="toggle__button"
  >
    <span v-if="isActive" class="toggle__label"></span>
    <span v-if="!isActive" class="toggle__label"></span>

    <input
      type="checkbox"
      :disabled="disabled"
      :id="id + '_button'"
      v-model="checkedValue"
    />
    <span class="toggle__switch"></span>
  </label>
</template>

<script>
export default {
  props: {
    disabled: {
      type: Boolean,
      default: false,
    },

    id: {
      type: String,
      default: "primary",
    },

    defaultState: {
      type: Boolean,
      default: false,
    },
  },

  data() {
    return {
      currentState: this.defaultState,
    };
  },

  watch: {
    defaultState: function defaultState() {
      this.currentState = Boolean(this.defaultState);
    },
  },

  computed: {
    // currentState() {
    //     return this.defaultState;
    // },

    isActive() {
      return this.currentState;
    },

    checkedValue: {
      get() {
        return this.currentState;
      },

      set(newValue) {
        this.currentState = newValue;
        this.$emit("change", newValue);
      },
    },
  },
};
</script>

<style scoped>
.toggle__button {
  vertical-align: middle;
  user-select: none;
  cursor: pointer;
  display: flex;
  margin-top: 1px;
}
.toggle__button input[type="checkbox"] {
  opacity: 0;
  position: absolute;
  width: 1px;
  height: 1px;
}
.toggle__button .toggle__switch {
  display: inline-block;
  height: 25px;
  border-radius: 12px;
  width: 45px;
  background: #d1d5db;
  box-shadow: inset 0 0 1px #e5e7eb;
  position: relative;
  transition: all 0.25s;
}

.toggle__button .toggle__switch::after,
.toggle__button .toggle__switch::before {
  content: "";
  position: absolute;
  display: block;
  height: 23px;
  width: 23px;
  border-radius: 50%;
  left: 1px;
  top: 1px;
  transform: translateX(0);
  transition: all 0.25s cubic-bezier(0.5, -0.6, 0.5, 1.6);
}

.toggle__button .toggle__switch::after {
  background: #fff;
  box-shadow: 0 0 1px #666;
}
.toggle__button .toggle__switch::before {
  background: #fff;
  box-shadow: 0 0 0 3px rgba(0, 0, 0, 0.1);
  opacity: 0;
}

.active .toggle__switch {
  background: #10b981;
  box-shadow: inset 0 0 1px #fff;
}

.active .toggle__switch::after,
.active .toggle__switch::before {
  transform: translateX(21px);
}

.active .toggle__switch::after {
  left: 0px;
  background: #fff;
  box-shadow: 0 0 1px #666;
}
</style>
