<template lang="pug">
  .basic-input(:class="`basic-input_${theme}`")
    input.basic-input__control(
      :type="type"
      :value="value"
      :required="required"
      :class="inputClass"
      v-on="listeners"
    )
    .basic-input__icon(v-if="icon")
      icon(:name="icon")
    label.basic-input__label {{ label }}
    transition(name="slide-up")
      .basic-input__error(v-if="errorMessage") {{ errorMessage }}
</template>

<script>
  import Icon from './Icon.vue';
  
  export default {
    components: {
      Icon,
    },
    props: {
      value: {
        type: String,
        default: '',
      },
      errorMessage: {
        type: String,
        default: '',
      },
      required: {
        type: Boolean,
        default: false,
      },
      disabled: {
        type: Boolean,
        default: false,
      },
      icon: {
        type: String,
        default: '',
      },
      label: {
        type: String,
        default: '',
      },
      theme: {
        type: String,
        default: 'dark',
      },
      type: {
        type: String,
        default: 'text',
      },
    },
    computed: {
      listeners() {
        return {
          ...this.$listeners,
          input: (event) => {
            if (!this.disabled) {
              this.$emit('input', event.target.value);
            }
          },
        };
      },
      inputClass() {
        return {
          'basic-input__control_dirty': this.value.length,
          'basic-input__control_disabled': this.disabled,
          'basic-input__control_invalid': this.errorMessage,
        };
      },
    },
  };
</script>

<style lang="postcss" scoped>
  @import '../../styles/mixins.pcss';

  .basic-input {
    position: relative;
    width: 100%;
    
    &__control {
      padding-left: 45px;
      width: 100%;
      color: $text-color;
      font-size: 18px;
      font-weight: 700;
      line-height: 56px;
      border-radius: 0;
      background: none;
      border: none;
      border-bottom: 1px solid $text-color;
      outline: none;
      transition: border 0.2s ease;
      &:focus {
        border-bottom-color: $links-color;
      }
      &_invalid,
      &_invalid:focus {
        border-bottom-color: $danger-color;
      }
    }

    &_light &__control {
      color: $text-color;
    }

    &__icon {
      position: absolute;
      top: 50%;
      left: 0;
      width: 30px;
      height: 30px;
      fill: rgba(70, 77, 98, 0.3);
      transform: translateY(-50%);
      pointer-events: none;
    }

    &_light &__icon {
      fill: rgba(70, 77, 98, 0.3);
    }

    &__control:focus ~ &__icon,
    &__control_dirty ~ &__label {
      fill: $links-color;
    }

    &__control_invalid ~ &__icon,
    &__control_invalid:focus ~ &__icon {
      fill: $danger-color;
    }

    &__label {
      position: absolute;
      top: 50%;
      left: 45px;
      bottom: auto;
      color: rgba(70, 77, 98, 0.3);
      font-size: 16px;
      font-weight: 600;
      line-height: 30px;
      transform: translateY(-50%);
      transition: transform 0.2s ease;
      pointer-events: none;
      @include desktop {
        font-size: 14px;
      }
    }

    &_light &__label {
      color: rgba(70, 77, 98, 0.3);
    }

    &__control:focus ~ &__label,
    &__control_dirty ~ &__label {
      transform: translateY(-200%);
    }

    &__error {
      position: absolute;
      z-index: 1;
      top: 100%;
      left: 45px;
      padding: 0 20px;
      color: white;
      background-color: $danger-color;
      font-size: 14px;
      font-weight: 400;
      line-height: 48px;
      z-index: 100;
      
      &::before,
      &::after {
        content: '';
        position: absolute;
        bottom: 100%;
        left: 50%;
        border: 3px solid $danger-color;
        border-left-width: 5px;
        border-right-width: 5px;
      }

      &::before {
        border-top-color: transparent;
        border-left-color: transparent;
        transform: translateX(-100%);
      }
      
      &::after {
        border-top-color: transparent;
        border-right-color: transparent;
      }
    }
  }

  .slide-up {
    &-enter-to,
    &-leave {
      transition: all 0.3s ease;
    }
    
    &-enter,
    &-leave-to {
      transform: translateY(50%);
      opacity: 0;
    }
  }
</style>