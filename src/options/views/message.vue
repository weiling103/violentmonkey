<template>
  <div class="message">
    <div class="mb-1" v-if="message.text" v-text="message.text"></div>
    <form v-if="message.buttons" @submit.prevent>
      <input class="mb-1" type="text" v-if="message.input !== false" v-model="message.input">
      <div>
        <button v-for="button in message.buttons" class="mr-1"
          :type="button.type || 'button'" v-text="button.text"
          @click="onButtonClick(button)"></button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: ['message'],
  mounted() {
    const input = this.$el.querySelector('input');
    if (input) input.focus();
  },
  methods: {
    onButtonClick(button) {
      const { onClick } = button;
      if (onClick) {
        if (onClick(this.message.input) !== false) this.dismiss();
      }
    },
    onBackdropClick() {
      const { onBackdropClick } = this.message;
      if (onBackdropClick) {
        if (onBackdropClick() !== false) this.dismiss();
      }
    },
    dismiss() {
      this.$emit('dismiss');
    },
  },
};
</script>

<style>
.message {
  width: 18rem;
  padding: 1rem;
  background: white;
  border-bottom-left-radius: .2rem;
  border-bottom-right-radius: .2rem;
  box-shadow: 0 0 .2rem rgba(0,0,0,.2);
}
.in-out {
  &-appear,
  &-enter,
  &-leave-active {
    .vl-modal-content > * {
      transform: translateY(-120%);
    }
    .vl-modal-backdrop {
      opacity: 0;
    }
  }
  &-appear-active,
  &-enter-active,
  &-leave-active {
    &,
    .vl-modal-content > * {
      transition: transform .5s;
    }
    .vl-modal-backdrop {
      transition: opacity .5s;
    }
  }
}
</style>
