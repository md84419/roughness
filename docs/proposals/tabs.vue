<script lang="ts" setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import { RTabItem, RTabs, RCard } from 'roughness';
import { WiredTabs, WiredTab } from "wired-elements";

const applicationSelectedTab = ref('Apps');
const wiredTabs = ref<HTMLElement | null>(null);

function handleWiredTabSelected(e: Event) {
  console.log('wired-tabs selected event:', e);
  const { detail } = e as CustomEvent<{ name?: string }>;
  // Some wired-tabs builds emit selected with detail.tab, others emit detail.name
  const newValue = detail?.name ?? detail?.tab;
  if (newValue) {
    console.log('Tab selected:', newValue);
    applicationSelectedTab.value = newValue;
  }
}

function syncFromWired() {
  const el = wiredTabs.value as any;
  if (el?.selected && el.selected !== applicationSelectedTab.value) {
    applicationSelectedTab.value = el.selected;
  }
}

onMounted(() => {
  wiredTabs.value?.addEventListener('selected', handleWiredTabSelected);
});

onBeforeUnmount(() => {
  wiredTabs.value?.removeEventListener('selected', handleWiredTabSelected);
});
</script>


<template>
<ClientOnly>
<wired-tabs
    fill="#e000e0"
    ref="wiredTabs"
    :selected.prop="applicationSelectedTab"
    @click="syncFromWired"
    @keydown="syncFromWired">
  <wired-tab name="Apps"></wired-tab>
  <wired-tab name="Logs"></wired-tab>
  <wired-tab name="Connections"></wired-tab>
</wired-tabs>
</ClientOnly>
<RTabs :model-value="applicationSelectedTab" @update:modelValue="applicationSelectedTab = $event">
  <RTabItem value="Apps">
    <RCard>
    <h2>Card 1</h2>
      <i>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</i>
    </RCard>
  </RTabItem>
  <RTabItem value="Logs">
    <RCard>
    <h2>Card 2</h2>
      <i>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</i>
    </RCard>
  </RTabItem>
  <RTabItem value="Connections">
    <RCard>
    <h2>Card 3</h2>
      <i>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</i>
    </RCard>
  </RTabItem>
</RTabs>
</template>



<style scoped>
  body {
    margin: 0;
    padding: 10px;
    line-height: 1.5;
  }

  .card-grid {
    display: grid;
    gap: 30px;
    grid-template-columns: repeat(3, 1fr);
  }

  wired-tabs {
    --wired-item-selected-color : var(--vp-c-bg);
    --wired-item-selected-bg: var(--vp-c-text-1);
  }

  wired-tabs {
    font-family: 'Arial Black';
    font-weight: 900;
  }
  
  wired-tab {
    display: none;
  }

  wired-card {
    padding: 20px;
    display: block;
  }

  main, section {
    max-width: 800px;
    margin: 0 auto;
    padding: 60px 0;
  }

  article > section > h2 {
    margin-top: 0;
  }

  wired-input {
    width: 200px;
    font-family: inherit;
  }
</style>

<style>
  .r-space.r-tabs__anchors {
    display: none;
  }
</style>