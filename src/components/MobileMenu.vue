<template>
  <div class="mobile-menu-wrapper">
    <div class="mobile-menu-bar shadow-far acrylic">
      <div class="id-sensei flex-horizontal">
        <div class="avatar-wrapper center">
          <img src="/src/assets/arona_icon.webp" alt="Arona" />
        </div>
        <div class="welcome-message">
          <input
            class="username"
            type="text"
            :value="username"
            @change="updateUsername"
          />
          <span>老师</span>
        </div>
      </div>

      <div
        class="button"
        role="button"
        tabindex="0"
        @click="toggleMenu"
        @keydown.space="toggleMenu"
        @keydown.enter="toggleMenu"
        @keydown.esc="closeMenu"
      >
        <span class="menu-line start" :class="showMenu ? 'active' : ''"></span>
        <span class="menu-line middle" :class="showMenu ? 'active' : ''"></span>
        <span class="menu-line end" :class="showMenu ? 'active' : ''"></span>
      </div>
    </div>

    <transition name="mask-transition">
      <div v-if="showMenu" class="submenu-controller" @click="closeMenu"></div>
    </transition>

    <transition name="menu-transition">
      <div
        class="mobile-submenu-container acrylic shadow-near"
        v-show="showMenu"
      >
        <div class="mobile-submenu">
          <router-link
            class="nav-link level-1 link-home rounded-small"
            to="/"
            @click="closeMenu"
            >Home
          </router-link>
          <router-link
            class="nav-link level-1 rounded-small"
            to="/mainStory"
            @click="closeMenu"
            >主线剧情
          </router-link>
          <router-link
            class="nav-link level-1 rounded-small"
            to="/miniStory"
            @click="closeMenu"
            >Mini Story
          </router-link>
          <router-link
            class="nav-link level-1 rounded-small"
            to="/groupStory"
            @click="closeMenu"
            >社团剧情
          </router-link>
          <router-link
            class="nav-link level-1 rounded-small"
            to="/archive"
            @click="closeMenu"
            >学生个人剧情
          </router-link>
          <router-link
            class="nav-link level-1 rounded-small"
            to="/contributors"
            @click="closeMenu"
            >成为贡献者
          </router-link>

          <div class="mobile-settings-container">
            <language-selector />
            <theme-switcher />
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue';
import { useSettingsStore } from '../store/settings';
import LanguageSelector from './LanguageSelector.vue';
import ThemeSwitcher from './ThemeSwitcher.vue';

const settingsStore = useSettingsStore();
const showMenu = ref(false);
const username = computed(() => settingsStore.getUsername);

function toggleMenu() {
  showMenu.value = !showMenu.value;
}

function closeMenu() {
  showMenu.value = false;
}

function updateUsername(event: Event) {
  const target = event.target as HTMLInputElement;
  const value = target.value.slice(0, 8);
  settingsStore.setUsername(value);
  target.style.width = `${Math.min(value.length, 8)}rem`;
}
</script>

<style scoped lang="scss">
.mobile-menu-wrapper {
  display: flex;
  flex-direction: column;
}

.mobile-menu-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: var(--color-mobile-nav-background);
  padding: 0.5rem 1rem;
}

.avatar-wrapper {
  backdrop-filter: brightness(0.8) saturate(0.8);
  -webkit-backdrop-filter: brightness(0.95) saturate(0.8);
  border: 0.1rem var(--color-player-avatar-border) solid;
  border-radius: 50%;
  -webkit-border-radius: 50%;
  padding: 0.1rem;

  img {
    border-radius: 50%;
    -webkit-border-radius: 50%;
    width: 2rem;
  }
}

.welcome-message {
  margin-left: 0.5rem;
}

.button {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  height: 1.2rem;
}

.menu-line {
  display: block;
  transition: all 0.375s ease-in-out;
  background-color: var(--color-text-main);
  width: 1.5rem;
  height: 2px;
  user-select: none;
  -webkit-user-select: none;

  &.active {
    &.start {
      transform: rotate3d(0, 0, 1, 45deg)
        translate3d(
          var(--trigo-param-positive, 10.608px),
          var(--trigo-param-positive, 10.608px),
          0
        )
        scale3d(1.414, 1, 1);
    }

    &.middle {
      transform: scale3d(0, 0, 0);
      opacity: 0;
    }

    &.end {
      transform: rotate3d(0, 0, 1, -45deg)
        translate3d(
          var(--trigo-param-positive, 10.608px),
          var(--trigo-param-negative, -10.608px),
          0
        )
        scale3d(1.414, 1, 1);
    }
  }
}

.mobile-submenu-container {
  z-index: -1;
  color: var(--color-text-main);

  .mobile-submenu {
    display: flex;
    flex-direction: column;
    background-color: var(--color-mobile-subnav-background);

    a {
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 0;
      padding: 0.5rem 1rem;
      width: 100%;
      color: var(--color-text-main);
      text-decoration: none;
    }
  }

  .mobile-settings-container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 0.5rem 1rem;
  }
}

.submenu-controller {
  position: absolute;
  top: 0;
  left: 0;
  opacity: 0.2;
  z-index: -1;
  background-color: #000;
  width: 100vw;
  height: 100vh;
}

.menu-transition-enter-active,
.menu-transition-leave-active {
  transition: all 0.375s ease;
}

.menu-transition-enter-from,
.menu-transition-leave-to {
  transform: translate3d(0, -100%, 0);
  opacity: 0;
}

.mask-transition-enter-active,
.mask-transition-leave-active {
  transition: all 0.375s ease;
}

.mask-transition-enter-from,
.mask-transition-leave-to {
  opacity: 0;
}

@media screen and (prefers-reduced-motion: reduce) {
  .menu-transition-enter-from,
  .menu-transition-leave-to {
    transform: none;
    opacity: 0;
  }
}
</style>
