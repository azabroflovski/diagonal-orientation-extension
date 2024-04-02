<script lang="ts" setup>
import { ref, watch } from 'vue'
import browser from 'webextension-polyfill'

const enabled = ref(false)

watch(enabled,async (value) => {
  const [tab] = await browser.tabs.query({
    active: true,
    currentWindow: true,
  })

  if (value) {
    await browser.scripting.executeScript({
      target: {
        tabId: tab.id!,
      },
      func: () => {
        document.documentElement.style.transition = '250ms ease'
        document.documentElement.style.transformOrigin = 'top left'
        document.documentElement.style.paddingTop = '40%'
        document.documentElement.style.transform = 'rotate(-22.44deg) translateX(-13%)'
      }
    })
  } else {
    await browser.scripting.executeScript({
      target: {
        tabId: tab.id!,
      },
      func: () => {
        document.documentElement.style.paddingTop = '0'
        document.documentElement.style.transform = 'rotate(0deg)'
      }
    })
  }
})
</script>

<template>
  <div class="popup">
    <h1 style="margin-bottom: 16px">Diagonal screen orientation</h1>

    <label class="toggle" style="margin-bottom: 16px">
      <input v-model="enabled" class="toggle-checkbox" type="checkbox">
      <div class="toggle-switch"></div>
      <span class="toggle-label">{{ enabled ? 'Enabled' : 'Disabled' }}</span>
    </label>

    <p>Why? <a href="https://habr.com/ru/articles/804371/" target="_blank">Read this</a></p>
  </div>
</template>

<style>
html,
body {
  width: 300px;
  padding: 10px;
  margin: 0;
}

body {
  background-color: rgb(36, 36, 36);
}

.popup p {
  color: #fff;
  font-size: 12px;
}

.popup a {
  color: #fff;
}

img {
  width: 200px;
  height: 200px;
}

h1 {
  font-size: 18px;
  color: white;
  font-weight: bold;
  margin: 0;
}

p {
  color: white;
  opacity: 0.7;
  margin: 0;
}

code {
  font-size: 12px;
  padding: 2px 4px;
  background-color: #ffffff24;
  border-radius: 2px;
}

*,
*:before,
*:after {
  box-sizing: border-box;
}

body {
  font-family: -apple-system, ".SFNSText-Regular", "Helvetica Neue", "Roboto", "Segoe UI", sans-serif;
}

.toggle {
  cursor: pointer;
  display: inline-block;
}

.toggle-switch {
  display: inline-block;
  background: #ccc;
  border-radius: 16px;
  width: 58px;
  height: 32px;
  position: relative;
  vertical-align: middle;
  transition: background 0.25s;
}

.toggle-switch:before, .toggle-switch:after {
  content: "";
}

.toggle-switch:before {
  display: block;
  background: linear-gradient(to bottom, #fff 0%, #eee 100%);
  border-radius: 50%;
  box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.25);
  width: 24px;
  height: 24px;
  position: absolute;
  top: 4px;
  left: 4px;
  transition: left 0.25s;
}

.toggle:hover .toggle-switch:before {
  background: linear-gradient(to bottom, #fff 0%, #fff 100%);
  box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.5);
}

.toggle-checkbox:checked + .toggle-switch {
  background: #56c080;
}

.toggle-checkbox:checked + .toggle-switch:before {
  left: 30px;
}

.toggle-checkbox {
  position: absolute;
  visibility: hidden;
}

.toggle-label {
  margin-left: 16px;
  position: relative;
  top: 2px;
  color: #fff;
  font-size: 18px;
}
</style>
