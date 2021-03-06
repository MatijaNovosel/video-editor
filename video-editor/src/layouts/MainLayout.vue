<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated>
      <q-bar class="bg-dark">
        <div class="q-ml-sm row">
          <div
            class="cursor-pointer q-mr-md bar-text"
            v-for="(action, i) in actions"
            :key="i"
            @click="action.onClick && action.onClick()"
          >
            {{ action.text }}
            <SubAction :actions="action.subActions || []" />
          </div>
        </div>
        <q-space />
        <span class="q-mx-sm text-bold current-project-title">
          {{ currentProjectTitle }}
        </span>
      </q-bar>
    </q-header>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script lang="ts">
import { Action } from "src/models/global";
import { defineComponent, computed } from "vue";
import SubAction from "src/components/global/SubAction.vue";
import { useStore, Getters } from "src/store";
import { NotificationService } from "src/services/notification/notification";
import { useMagicKeys, whenever } from "@vueuse/core";

export default defineComponent({
  name: "MainLayout",
  components: { SubAction },
  setup() {
    const store = useStore();
    const notificationService = new NotificationService();

    const actions: Action[] = [
      {
        text: "File",
        subActions: [
          {
            text: "New project",
            onClick: () => {
              notificationService.notify("New project has been created!");
            }
          },
          {
            text: "Close project"
          },
          {
            text: "Export"
          },
          {
            text: "Save"
          },
          {
            text: "Close"
          }
        ]
      },
      {
        text: "Edit",
        subActions: [
          {
            text: "Sublabel 1",
            actions: [
              {
                text: "Sublabel 2",
                actions: [{ text: "Sublabel #1" }, { text: "Sublabel #2" }]
              }
            ]
          }
        ]
      },
      {
        text: "View"
      },
      {
        text: "Window"
      },
      {
        text: "Help"
      }
    ];

    const { ctrl_s } = useMagicKeys();

    whenever(ctrl_s, () => {
      notificationService.notify("Project saved!");
    });

    return {
      actions,
      currentProjectTitle: computed(() => {
        return (store.getters as Getters)["app/currentProjectTitle"];
      })
    };
  }
});
</script>

<style>
.bar-text {
  font-size: 12px;
}

.current-project-title {
  font-size: 12px;
}
</style>
