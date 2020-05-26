<template>
  <div class="titlebar" @dblclick.prevent.stop="maximizeWindow" :class="{windows: isWindows}">
    <div class="titlebar-icon" v-if="!isMac">
      <img src="@/assets/logo.svg" />
      <template v-if="isWindows">
        <hsc-menu-style-white>
          <hsc-menu-bar style="border-radius: 0 0 4pt 0;">
            <hsc-menu-bar-item label="File">
              <hsc-menu-item label="New Query" />
              <hsc-menu-item label="New Connection" />
              <hsc-menu-separator/>
              <hsc-menu-item label="Preferences">
                <hsc-menu-item label="Settings" />
                <hsc-menu-item label="Keyboard Shortcuts" />
              </hsc-menu-item>
            </hsc-menu-bar-item>
            <hsc-menu-bar-item label="Edit">
              <hsc-menu-item label="Undo" keybind="meta+z" />
              <hsc-menu-separator/>
              <hsc-menu-item label="Cut" keybind="meta+x" />
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
