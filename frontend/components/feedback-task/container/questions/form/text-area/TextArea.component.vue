<template>
  <div class="wrapper">
    <QuestionHeaderComponent :question="question" />
    <TextAreaContents
      v-if="!question.suggestion"
      :question="question"
      :is-focused="isFocused"
    />
    <BaseCardWithTabs v-else :tabs="tabs">
      <template v-slot="{ currentComponent }">
        <component
          :question="question"
          :is-focused="isFocused"
          :is="currentComponent"
          :key="currentComponent"
        />
      </template>
    </BaseCardWithTabs>
  </div>
</template>

<script>
export default {
  name: "TextAreaComponent",
  props: {
    question: {
      type: Object,
      required: true,
    },
    isFocused: {
      type: Boolean,
      default: () => false,
    },
  },
  computed: {
    isSuggested() {
      return this.question.suggestion?.isSuggested(this.question.answer.value);
    },
    tabs() {
      return [
        {
          id: "0",
          name: this.isSuggested ? "Suggestion" : "Write",
          icon: this.isSuggested && "suggestion",
          component: "TextAreaContents",
        },
        ...(!this.isSuggested
          ? [
              {
                id: "1",
                name: "Suggestion",
                icon: "suggestion",
                component: "TextAreaSuggestion",
              },
            ]
          : []),
      ];
    },
  },
};
</script>

<style lang="scss" scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  gap: $base-space;
}
</style>
