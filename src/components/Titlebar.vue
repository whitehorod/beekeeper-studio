<template>
  <div class="titlebar" @dblclick.prevent.stop="maximizeWindow" :class="{windows: isWindows}">
    <div class="titlebar-icon" v-if="!isMac">
      <img src="@/assets/logo.svg" />
      <template>
        <hsc-menu-style-white v-if="isWindows">
          <hsc-menu-bar style="border-radius: 0 0 4pt 0;">
            <hsc-menu-bar-item label="File">
              <hsc-menu-item label="New" @click="window.alert('New')" />
              <hsc-menu-item label="Open" @click="window.alert('Open')" />
              <hsc-menu-separator/>
              <hsc-menu-item label="Save" @click="window.alert('Save')" :disabled="true" />
              <hsc-menu-item label="Export to">
                <hsc-menu-item label="PDF" />
                <hsc-menu-item label="HTML" />
              </hsc-menu-item>
            </hsc-menu-bar-item>
            <hsc-menu-bar-item label="Edit">
              <hsc-menu-item label="Undo" keybind="meta+z" @click="window.alert('Undo')" />
              <hsc-menu-separator/>
              <hsc-menu-item label="Cut" keybind="meta+x" @click="window.alert('Cut')" />
              <hsc-menu-item label="Copy" keybind="meta+c" @click="window.alert('Copy')" />
              <hsc-menu-item label="Paste" keybind="meta+v" @click="window.alert('Paste')" :disabled="true" />
            </hsc-menu-bar-item>
          </hsc-menu-bar>
        </hsc-menu-style-white>
      </template>
    </div>
    <div class="titlebar-title noselect">Beekeeper Studio</div>
    <div class="titlebar-actions" v-if="isWindows">
      <template>
        <button class="btn btn-link" id="minimize" @click.prevent="minimizeWindow"><i class="material-icons">remove</i></button>
        <button class="btn btn-link" id="maximize" @click.prevent="maximizeWindow">
          <i class="material-icons" v-if="maximized">filter_none</i>
          <i class="material-icons" v-else>crop_square</i>
        </button>
        <button class="btn btn-link" id="quit" @click.prevent="closeWindow"><i class="material-icons">clear</i></button>
      </template>
    </div>
  </div>
</template>

<script>
import {remote} from 'electron'
export default {
    data() {
      return {
        window: remote.getCurrentWindow(),
        maximized: remote.getCurrentWindow().isMaximized()
      }
    },
    mounted() {
      this.window.on('maximize', () => {
        this.maximized = true
      })

      this.window.on('unmaximize', () => {
        this.maximized = false
      })
    },
    methods: {
      minimizeWindow() {
        this.window.minimize();
      },
      maximizeWindow() {
        if (this.window.isMaximized()) {
          this.window.unmaximize();
        } else {
          this.window.maximize();
        }
      },
      closeWindow() {
        window.close()
      }
    }
  }
</script>

<style>
  #windows-title {
    user-select: none;
  }
</style>
