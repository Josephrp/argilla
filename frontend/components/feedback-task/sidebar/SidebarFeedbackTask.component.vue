<template>
  <div class="sidebar__container">
    <SidebarFeedbackTaskPanel v-if="isPanelVisible" @close-panel="closePanel">
      <HelpShortcut v-if="currentPanel === 'help-shortcut'" />
      <FeedbackTaskProgress
        v-else-if="currentPanel === 'metrics'"
        :datasetId="datasetId"
      />
    </SidebarFeedbackTaskPanel>
    <SidebarFeedbackTask
      @on-click-sidebar-action="onClickSidebarAction"
      :sidebar-items="sidebarItems"
      :active-buttons="[currentPanel, currentMode]"
      :expanded-component="currentPanel"
    />
  </div>
</template>

<script>
import "assets/icons/progress";
import "assets/icons/refresh";
import "assets/icons/shortcuts";

export default {
  props: {
    datasetId: {
      type: String,
      required: true,
    },
  },
  data: () => ({
    currentPanel: null,
    currentMode: "annotate",
    isPanelVisible: false,
  }),
  created() {
    this.sidebarItems = {
      firstGroup: {
        buttonType: "expandable",
        buttons: [
          {
            id: "metrics",
            tooltip: this.$t("sidebar.progressTooltip"),
            icon: "progress",
            action: "show-metrics",
            type: "expandable",
            component: "FeedbackTaskProgress",
          },
        ],
      },
      secondGroup: {
        buttonType: "default",
        buttons: [
          {
            id: "refresh",
            tooltip: this.$t("sidebar.refreshTooltip"),
            icon: "refresh",
            group: this.$t("refresh"),
            type: "non-expandable",
            action: "refresh",
          },
        ],
      },
      bottomGroup: {
        buttonType: "expandable",
        buttons: [
          {
            id: "help-shortcut",
            tooltip: this.$t("sidebar.shortcutsTooltip"),
            icon: "shortcuts",
            action: "show-help",
            type: "custom-expandable",
            component: "HelpShortcut",
          },
        ],
      },
    };
  },
  methods: {
    onClickSidebarAction(group, info) {
      switch (group.toUpperCase()) {
        case "FIRSTGROUP":
          this.togglePanel(info);
          break;
        case "SECONDGROUP":
          this.$emit("refresh");
          break;
        case "BOTTOMGROUP":
          this.togglePanel(info);
          break;
        default:
          console.warn(info);
      }
    },
    togglePanel(panelContent) {
      this.currentPanel =
        this.currentPanel !== panelContent ? panelContent : null;

      this.isPanelVisible = !!this.currentPanel;

      $nuxt.$emit("on-sidebar-toggle-panel", this.isPanelVisible);
    },
    closePanel() {
      this.isPanelVisible = false;
      this.currentPanel = null;
      $nuxt.$emit("on-sidebar-toggle-panel", null);
    },
  },
};
</script>

<style lang="scss" scoped>
.sidebar {
  &__container {
    position: fixed;
    display: flex;
    right: 0;
    z-index: 1;
    pointer-events: none;
    @include media("<=tablet") {
      display: none;
    }
  }
}
</style>
