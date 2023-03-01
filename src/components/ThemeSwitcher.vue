<template>
  <button class="toggle-button" @click.prevent="applySetting(toggleSetting());">
    <template v-if="state.currentSetting === 'light'">
      <span class="toggle-button__label">DARK</span>
      <IconSwitchDark />
    </template>
    <template v-if="state.currentSetting === 'dark'">
      <span class="toggle-button__label">LIGHT</span>
      <IconSwitchLight />
    </template>
  </button>
</template>

<script setup>
import IconSwitchDark from '@/components/icons/IconSwitchDark.vue';
import IconSwitchLight from '@/components/icons/IconSwitchLight.vue';
import { reactive } from 'vue';

const STORAGE_KEY = 'user-color-scheme';
const COLOR_MODE_KEY = '--color-mode';

const state = reactive( { currentSetting: '' })


const getCSSCustomProp = propKey => {
  let response = getComputedStyle(document.documentElement).getPropertyValue(propKey);

  if (response.length) {
    response = response.replace(/"/g, '').trim();
  }
  return response;
};

const applySetting = passedSetting => {
  state.currentSetting = passedSetting || localStorage.getItem(STORAGE_KEY);

  if (state.currentSetting) {
    document.documentElement.setAttribute('data-user-color-scheme', state.currentSetting);
  }
};


const toggleSetting = () => {
  state.currentSetting = localStorage.getItem(STORAGE_KEY);

  switch (state.currentSetting) {
    case null:
      state.currentSetting = getCSSCustomProp(COLOR_MODE_KEY) === 'dark' ? 'light' : 'dark';
      break;
    case 'light':
      state.currentSetting = 'dark';
      break;
    case 'dark':
      state.currentSetting = 'light';
      break;
  }

  localStorage.setItem(STORAGE_KEY, state.currentSetting);

  return state.currentSetting;
};

applySetting();


</script>

<style scoped>
.toggle-button {
  align-items: center;
  background: transparent;
  block-size: 20px;
  border: none;
  color: var(--text-color-light);
  column-gap: 1rem;
  display: flex;
  font-family: "Space Mono", monospace;
  padding: 0;

  & .toggle-button__label {
    font-size: 13px;
    font-weight: 700;
    font-variant: all-small-caps;
    letter-spacing: 3px;
  }

  &:hover {
      color: var(--toggle-button-hover);
    }
  }


</style>
