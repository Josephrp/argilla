<template>
  <component
    :class="[
      clickable ? 'badge--clickable' : 'badge',
      activeBadge ? 'badge--active' : 'badge',
    ]"
    :is="renderComponent"
    @click="onClick($event)"
    ><span class="badge__text">{{ text }}</span>
  </component>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
      required: true,
    },
    activeBadge: {
      type: Boolean,
      default: false,
    },
    color: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      renderComponent: "p",
      clickable: false,
      clearable: false,
    };
  },
  mounted() {
    if (this.$listeners["on-click"]) {
      this.clickable = true;
      this.renderComponent = "baseButton";
    }
    if (this.$listeners["on-clear"]) {
      this.clearable = true;
    }
  },
  methods: {
    onClick($event) {
      this.$emit("on-click", $event);
    },
    onClear($event) {
      this.$emit("on-clear", $event);
    },
  },
};
</script>

<style lang="scss" scoped>
.badge {
  display: inline-flex;
  align-items: center;
  gap: $base-space * 1.3;
  max-width: 200px;
  margin: 0 auto 0 0;
  margin-left: 0;
  padding: 2px calc($base-space / 2);
  border-radius: 2px;
  background-color: v-bind(color);
  color: $black-87;
  font-family: "Roboto Condensed", sans-serif;
  font-weight: 500;
  @include font-size(12px);
  @include line-height(12px);
  text-transform: uppercase;
  &--clickable {
    @extend .badge;
    &:not(.badge--active):hover {
      opacity: 0.8;
    }
  }
  &--active {
    @extend .badge;
    opacity: 0.8;
  }
  &__text {
    @include truncate;
  }
}
</style>
