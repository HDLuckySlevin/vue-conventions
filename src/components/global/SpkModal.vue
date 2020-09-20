<template>
  <transition name="modal">
    <div class="modal">
      <div class="modal__container" :class="[isFullwidth ? 'modal__container--isFullwidth' : '']">
        <spk-button
          v-if="!isNotCloseable"
          class="modal__close-button button button--ghost"
          :class="modalState ? 'color-light' : 'color-dark'"
          @click.native="closeModal()"
        >
          <spk-icon iconClass="icon--cross" iconName="cross" />
        </spk-button>
        <div class="modal__header p-l-s p-r-xxl" :class="modalState ? 'bg-' + modalState : ''">
          <slot name="header">
            <h3>
              Default header
            </h3>
          </slot>
        </div>
        <div class="modal__body p-s m-v-l">
          <slot name="body">
            <span>Default body</span>
          </slot>
        </div>
        <div class="modal__footer p-s">
          <slot name="footer">
            <spk-button v-if="!isNotCloseable" class="button--first" @click.native="closeModal()">
              Weiter
            </spk-button>
          </slot>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import Vue from 'vue';

export default Vue.extend({
  name: 'SpkModal',
  props: {
    modalVisible: {
      type: Boolean,
      default: false
    },
    isNotCloseable: {
      type: Boolean,
      required: false,
      default: false
    },
    isFullwidth: {
      type: Boolean,
      required: false,
      default: false
    },
    modalState: {
      type: String
    }
  },
  methods: {
    closeModal() {
      this.$emit('close');
    }
  }
});
</script>

<style scoped lang="scss">
.modal {
  position: fixed;
  z-index: z('modal');
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(map-get($colors, 'dark'), 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: opacity 0.3s ease;

  &__container {
    position: relative;
    width: 20vw;
    margin: 0 auto;
    transition: all 0.3s ease;
    background: map-get($colors, 'light');

    &--isFullwidth {
      width: 95vw;
    }
  }

  &__close-button {
    position: absolute;
    right: 0;
    top: 4px;
  }

  &__header {
    margin-top: 0;
  }

  &__body {
    margin: 20px 0;
  }
}

/*
   * The following styles are auto-applied to elements with
   * transition="modal" when their visibility is toggled
   * by Vue.js.
   *
   * You can easily play with the modal transition by editing
   * these styles.
   */

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>