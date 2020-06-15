<template>
  <modal name="settings-modal" transition="pop-out" :width="600" :pivot-y="0.4" :height="340">
      <div class="settings-modal-title flex-center">
        Settings
      </div>
      <div class="settings-modal-content">
        <div class="btn-group flex-item">
          <b>Font:</b>
          <select @change="changeFont">
            <option v-for="font in fontOptions" :value="font" :selected="setDefault('font', font)"> {{font}} </option>
          </select>
        </div>
        <div class="btn-group flex-item">
          <b>Font Size:</b>
          <select @change="changeSize">
            <option v-for="size in sizeOptions" :value="size" :selected="setDefault('size', size)">{{size}}</option>
          </select>
        </div>
        <div class="btn-group flex-item">
          <b>Tab Size:</b>
          <select @change="changeTabSize">
            <option v-for="tabSize in tabSizeOptions" :value="tabSize" :selected="setDefault('tabSize', tabSize)">{{tabSize}}</option>
          </select>
        </div>
        <div class="btn-group flex-item">
          <b>Theme:</b>
          <select @change="changeTheme">
           <option v-for="theme in themeOptions" :value="theme" :selected="setDefault('theme', theme)">{{themeOptionsMap[theme]}}</option>
          </select>
        </div>
        <div class="btn-group flex-item">
          <b>Word Wrap:</b>
          <select @change="changeWordWrap">
            <option v-for="wordWrap in wordWrapOptions" :value="wordWrap" :selected="setDefault('wordWrap', wordWrap)"> {{wordWrap}} </option>
          </select>
        </div>
      </div>
      <div class="settings-modal-footer">
          <ul class="list-inline panel-actions">
            <li @click="resetEditor"><a href="#">Reset Defaults</a></li>
            <li><button class="btn btn-run btn-sm" @click="closeSettingsModal()">
                Close
              </button>
            </li>
          </ul>
      </div>
    </modal>
</template>

<script>
  export default {
    name: 'settings',
    data () {
      return {
        themeOptions: ['vs-dark', 'vs', 'hc-black'],
        themeOptionsMap: {
          'vs-dark': 'Visual Studio Dark',
          'vs': 'Visual Studio',
          'hc-black': 'High Contract Dark'
        },
        fontOptions: ['Lucida Console', 'Anonymous Pro', 'Courier', 'Droid Sans Mono', 'Inconsolata', 'Source Code Pro', 'Ubuntu Mono'],
        sizeOptions: Array(30).fill(0).map((el, ind) => 6 + (2 * ind)),
        tabSizeOptions: [2, 4, 8],
        wordWrapOptions: ['on', 'off']
      }
    },
    methods: {
      changeTheme (e) {
        this.$store.commit('changeTheme', e.target.value)
      },
      changeFont (e) {
        this.$store.commit('changeFont', e.target.value)
      },
      changeSize (e) {
        this.$store.commit('changeFontSize', e.target.value)
      },
      changeTabSize (e) {
        this.$store.commit('changeTabSize', e.target.value)
      },
      changeWordWrap (e) {
        this.$store.commit('changeWordWrap', e.target.value)
      },
      resetEditor () {
        this.$store.commit('resetEditor')
      },
      closeSettingsModal() {
        this.$modal.hide('settings-modal')
      },
      setDefault (type, val) {
        switch (type) {
          case 'theme':
            return val === this.$store.state.theme
          case 'size':
            return val === parseInt(this.$store.state.fontSize)
          case 'tabSize':
            return val === parseInt(this.$store.state.tabSize)
          case 'font':
            return val === this.$store.state.font
          case 'wordWrap':
            return val === this.$store.state.wordWrap
        }
      }
    }
  }
</script>

<style scoped>
  .btn-group, .panel-actions {
    margin: 10px;
  }

  .settings-modal-footer .panel-actions button{
    float: right;
    width: 80px;
  }

  .settings-modal-footer {
    margin: 0px 10px 10px;
    padding: 10px 10px 10px 0px;
    color: #b4b4b4;
  }

  .settings-modal-footer a {
    color:#fff;
  }

  .settings-modal-content select {
    z-index: 20;
    color: #202020;
    background: white !important;
    border-radius: 4px;
    overflow: hidden;
    height: 28px;
    padding: 4px;
    margin-left: 8px;
    border: none;
  }

  .settings-modal-content select:focus {
    outline: none;
  }
  
</style>
