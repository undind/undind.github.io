<template lang="pug">
  button.card-button(
    :class="{ 'card-button__disabled': isDisabled }"
    v-on="$listeners"
  )
    .card-button__icon
    .card-button__text
      slot
</template>

<script>
export default {
  props: {
    isDisabled: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="postcss" scoped>
@import '../../styles/mixins.pcss';

.card-button {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  padding: 30px;
  background-image: $admin-gradient;
  color: white;
  font-size: 18px;
  font-weight: 700;
  line-height: 30px;
  transition: opacity 0.2s ease;

  &:hover {
    opacity: 0.95;
  }
}

.card-button__disabled::after {
  position: absolute;
  content: '';
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(white, 0.7);
  cursor: default;
}

.card-button__icon {
  position: relative;
  width: 150px;
  height: 150px;
  border-radius: 50%;
  border: 2px solid #fff;

  &::before,
  &::after {
    position: absolute;
    content: '';
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
  }

  &::before {
    height: 34px;
    width: 2px;
  }

  &::after {
    width: 34px;
    height: 2px;
  }
}

.card-button__text {
  margin-top: 30px;
}
</style>