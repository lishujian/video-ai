<template>
  <div class="video-parser-container">
    <div class="main-content">
      <!-- Player Section -->
      <div class="player-wrapper">
        <div class="iframe-container">
          <iframe
            v-if="currentUrl"
            :src="parseUrl"
            allowfullscreen="true"
            webkitallowfullscreen="true"
            mozallowfullscreen="true"
            allowtransparency="true"
            frameborder="0"
            scrolling="no"
          ></iframe>
          <div v-else class="empty-player"></div>
        </div>
      </div>

      <!-- Controls Section -->
      <div class="controls-panel">
        <div class="input-group">
          <div class="interface-select-row">
            <span class="label-text">选择接口</span>
            <div class="custom-select" :class="{ 'is-open': dropdownOpen }" v-click-outside="() => dropdownOpen = false">
              <div class="select-trigger" @click="dropdownOpen = !dropdownOpen">
                <span class="selected-value">{{ interfaces[selectedInterface].name }}</span>
                <i class="arrow-icon"></i>
              </div>
              <transition name="fade-slide">
                <div v-if="dropdownOpen" class="select-options-list">
                  <div 
                    v-for="(item, index) in interfaces" 
                    :key="index" 
                    class="select-option"
                    :class="{ 'is-selected': selectedInterface === index }"
                    @click="selectInterface(index)"
                  >
                    {{ item.name }}
                  </div>
                </div>
              </transition>
            </div>
          </div>
          <div class="url-input-row">
            <input 
              v-model="videoUrl" 
              placeholder="电脑使用Ctrl+V粘贴网址-手机直接长按粘贴网址" 
              @keyup.enter="handleParse"
            />
          </div>
        </div>
        
        <button class="parse-button" @click="handleParse">立即解析</button>
      </div>

      <!-- Shortcut Platforms Section -->
      <div class="platforms-section">
        <div class="platforms-row">
          <button v-for="item in platforms.slice(0, 4)" :key="item.name" @click="openPlatform(item.url)">
            进入{{ item.name }}视频
          </button>
        </div>
        <div class="platforms-row">
          <button v-for="item in platforms.slice(4, 8)" :key="item.name" @click="openPlatform(item.url)">
            进入{{ item.name }}视频
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const videoUrl = ref('')
const currentUrl = ref('')
const selectedInterface = ref(0)
const dropdownOpen = ref(false)

const selectInterface = (index) => {
  selectedInterface.value = index
  dropdownOpen.value = false
}

// Simple Click-Outside Directive
const vClickOutside = {
  mounted(el, binding) {
    el._clickOutside = (event) => {
      if (!(el === event.target || el.contains(event.target))) {
        binding.value(event)
      }
    }
    document.addEventListener('click', el._clickOutside)
  },
  unmounted(el) {
    document.removeEventListener('click', el._clickOutside)
  }
}

const interfaces = [
  { name: 'OK', url: 'https://video.isyour.love/player/getplayer?url=' },
  { name: '云播全网解析', url: 'https://video.isyour.love/player/getplayer?url=' },
  { name: '多线路视频解析 [新]', url: 'https://jx.playerjy.com/?url=' },
  { name: '视频云解析 【全网解析】', url: 'https://jx.xymp4.cc/?url=' },
  { name: '无广告视频解析 【全网解析】', url: 'https://jx.m3u8.tv/jiexi/?url=' },
  { name: '输入片名自动解析 【新】', url: 'https://z1.im1907.top/?jx=' },
  { name: '直接搜片名专用', url: 'https://z1.m1907.top/?jx=' },
]

const platforms = [
  { name: '乐视', url: 'http://www.le.com/' },
  { name: '奇艺', url: 'http://www.iqiyi.com/' },
  { name: '芒果', url: 'http://www.mgtv.com/' },
  { name: '哔哩哔哩', url: 'http://www.bilibili.com/' },
  { name: '腾讯', url: 'http://v.qq.com/' },
  { name: '优酷', url: 'http://www.youku.com/' },
  { name: '搜狐', url: 'http://tv.sohu.com/' },
  { name: 'pptv', url: 'http://www.pptv.com/' },
]

const parseUrl = computed(() => {
  if (!currentUrl.value) return ''
  return interfaces[selectedInterface.value].url + currentUrl.value
})

const handleParse = () => {
  if (!videoUrl.value) {
    alert('请输入网址或片名')
    return
  }
  currentUrl.value = videoUrl.value
}

const openPlatform = (url) => {
  window.open(url, '_blank')
}
</script>

<style lang="less" scoped>
/* 轻暖极简风：奶油底 + 柔紫主按钮 + 清爽表单 */

@text: #374151;
@text-muted: #6b7280;
@card: #ffffff;
@card-border: #e9e5f2;
@input-bg: #fcfbff;
@input-border: #ddd6ea;
@primary: #9b7cf4;
@primary-hover: #8a67ec;
@primary-focus: rgba(155, 124, 244, 0.25);

.video-parser-container {
  min-height: 100vh;
  width: 100%;
  background: transparent;
  display: flex;
  justify-content: center;
  padding: 10px;
  box-sizing: border-box;
}

.main-content {
  width: 100%;
  max-width: 900px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.player-wrapper,
.controls-panel {
  background: @card;
  border: 1px solid @card-border;
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(125, 102, 184, 0.08);
}

.player-wrapper {
  padding: 12px;
}

.iframe-container {
  width: 100%;
  aspect-ratio: 16 / 9;
  position: relative;
  border-radius: 10px;
  overflow: hidden;
  border: 1px solid #d8dce5;
  background: #111318;
  display: flex;

  iframe,
  .empty-player {
    width: 100%;
    height: 100%;
    border: none;
    flex: 1;
  }
}

.empty-player {
  display: flex;
  align-items: center;
  justify-content: center;
  color: #9ca3af;
  font-size: 13px;

  &::after {
    content: '解析成功后在此播放';
  }
}

.controls-panel {
  position: relative;
  display: flex;
  flex-direction: column;
  z-index: 10;
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 14px;

  .interface-select-row {
    display: flex;
    min-height: 46px;
    border: 1px solid @input-border;
    border-radius: 10px;
    background: @input-bg;
    position: relative; // Ensure z-index works if needed

    .label-text {
      min-width: 92px;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 0 12px;
      background: #f5f1fb;
      color: @text-muted;
      font-size: 13px;
      border-right: 1px solid @input-border;
      border-radius: 9px 0 0 9px;
    }

    .custom-select {
      flex: 1;
      position: relative;
      cursor: pointer;
      user-select: none;

      .select-trigger {
        display: flex;
        align-items: center;
        justify-content: space-between;
        height: 46px;
        padding: 0 16px;
        color: @text;
        font-size: 14px;
        height: 100%;

        .selected-value {
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }

        .arrow-icon {
          width: 0;
          height: 0;
          border-left: 5px solid transparent;
          border-right: 5px solid transparent;
          border-top: 5px solid #8b95a8;
          transition: transform 0.2s ease;
        }
      }

      &.is-open .arrow-icon {
        transform: rotate(180deg);
      }

      .select-options-list {
        position: absolute;
        top: calc(100% + 6px);
        left: 0;
        right: 0;
        background: #ffffff;
        border: 1px solid @input-border;
        border-radius: 12px;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
        z-index: 100;
        padding: 6px;
        max-height: 280px;
        overflow-y: auto;

        .select-option {
          padding: 10px 14px;
          border-radius: 8px;
          color: @text;
          font-size: 13.5px;
          transition: all 0.15s ease;
          margin-bottom: 2px;

          &:last-child {
            margin-bottom: 0;
          }

          &:hover {
            background: #f3f0ff;
            color: @primary;
          }

          &.is-selected {
            background: @primary;
            color: #ffffff;
          }
        }
      }

      /* Scrollbar Styling for dropdown */
      .select-options-list::-webkit-scrollbar {
        width: 5px;
      }
      .select-options-list::-webkit-scrollbar-thumb {
        background: #ddd;
        border-radius: 10px;
      }
      .select-options-list::-webkit-scrollbar-track {
        background: transparent;
      }
    }
  }

  /* Animations for dropdown */
  .fade-slide-enter-active,
  .fade-slide-leave-active {
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .fade-slide-enter-from,
  .fade-slide-leave-to {
    opacity: 0;
    transform: translateY(-8px);
  }

  .url-input-row {
    input {
      width: 100%;
      box-sizing: border-box;
      min-height: 46px;
      border: 1px solid @input-border;
      border-radius: 10px;
      padding: 0 14px;
      font-size: 14px;
      color: @text;
      background: @input-bg;
      outline: none;
      transition: border-color 0.18s ease, box-shadow 0.18s ease;

      &::placeholder {
        color: #9ca3af;
      }

      &:hover {
        border-color: #cfc5e3;
      }

      &:focus {
        border-color: @primary;
        box-shadow: 0 0 0 3px @primary-focus;
      }
    }
  }
}

.parse-button {
  width: calc(100% - 28px);
  height: 46px;
  margin: 0 14px 14px;
  border: none;
  border-radius: 10px;
  background: @primary;
  color: #fff;
  font-size: 15px;
  font-weight: 600;
  letter-spacing: 0.01em;
  cursor: pointer;
  transition: background-color 0.18s ease, transform 0.12s ease, box-shadow 0.18s ease;
  box-shadow: 0 6px 16px rgba(155, 124, 244, 0.28);

  &:hover {
    background: @primary-hover;
    transform: translateY(-1px);
  }

  &:active {
    transform: translateY(0);
  }
}

.platforms-section {
  display: flex;
  flex-direction: column;
  gap: 10px;

  .platforms-row {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;

    button {
      background: #ffffff;
      color: #4b5563;
      border: 1px solid @card-border;
      border-radius: 10px;
      padding: 10px 8px;
      font-size: 13px;
      cursor: pointer;
      white-space: nowrap;
      transition: all 0.18s ease;

      &:hover {
        color: #5b3cc4;
        border-color: #ccbdf1;
        background: #fbf8ff;
        box-shadow: 0 6px 14px rgba(155, 124, 244, 0.14);
        transform: translateY(-1px);
      }
    }
  }
}

@media (max-width: 600px) {
  .input-group .interface-select-row {
    flex-direction: column;
    min-height: auto;
    height: auto;
    width: 100%;
    box-sizing: border-box;

    .label-text {
      width: 100%;
      box-sizing: border-box;
      min-width: 0;
      justify-content: flex-start;
      padding: 0 14px;
      height: 40px;
      border-right: none;
      border-bottom: 1px solid @input-border !important;
      border-radius: 9px 9px 0 0;
      color: @text-muted;
      font-weight: 500;
      font-size: 13px;
    }

    .custom-select {
      width: 100%;
      box-sizing: border-box;
      
      .select-trigger {
        height: 44px;
        padding: 0 14px;
        box-sizing: border-box;
        border-radius: 0 0 9px 9px;
      }
    }
  }

  .platforms-row {
    grid-template-columns: repeat(2, 1fr) !important;
  }
}
</style>
