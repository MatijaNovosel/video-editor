<template>
  <q-menu anchor="top end" self="top start" v-if="actions && actions.length !== 0">
    <q-list dense>
      <q-item
        v-for="(action, i) in actions"
        :key="`subAction_${i}`"
        clickable
        @click="action.onClick && action.onClick()"
      >
        <q-item-section class="text">
          {{ action.text }}
        </q-item-section>
        <q-item-section side v-if="action.actions && action.actions.length !== 0">
          <q-icon name="keyboard_arrow_right" />
        </q-item-section>
        <SubAction v-if="action.actions && action.actions.length !== 0" :actions="action.actions" />
      </q-item>
    </q-list>
  </q-menu>
</template>

<script lang="ts">
import { SubAction } from "src/models/global";
import { defineComponent, PropType } from "vue";

export default defineComponent({
  name: "SubAction",
  props: {
    actions: {
      type: Object as PropType<SubAction[]>,
      required: true
    }
  },
  setup() {
    return {};
  }
});
</script>

<style>
.text {
  font-size: 12px;
}
</style>
