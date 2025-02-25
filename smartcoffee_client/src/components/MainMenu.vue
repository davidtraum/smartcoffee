
<template>
  <ion-menu side="end" menu-id="main" content-id="main">
    <ion-header>
      <ion-toolbar color="primary">
        <ion-title> Beanü </ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <ion-list>
        <ion-item>
          <ion-icon class="menuIcon" :icon="person" />
          <ion-label>{{ username }}</ion-label>
        </ion-item>
        <ion-item>
          <ion-icon :icon="contrastSharp" class="menuIcon" />
          <ion-label>Dunkles Design</ion-label>
          <ion-toggle
            @ionChange="onToggleDarkmode($event)"
            :checked="DesignService.status.dark"
          />
        </ion-item>
        <ion-item @click="openSettings()">
          <ion-icon :icon="settingsSharp" class="menuIcon" />
          <ion-label expand="full"> Einstellungen</ion-label>
          <ion-ripple-effect />
        </ion-item>
      </ion-list>
    </ion-content>
    <ion-footer class="ion-no-border" ref="footer">
      <ion-img
        class="watermark"
        style="width: 125px"
        :class="{ 'move-out': !visible }"
        :src="require('@/assets/LogoWithHeader.png')"
      ></ion-img>
      <ion-text class="version">
        <p>Version 0.0.1</p>
      </ion-text>
    </ion-footer>
  </ion-menu>
</template>

<script lang="ts">
/**
@author dtraum
@date 26.11.2021
 */

import { defineComponent, ref } from "vue";

import {
  IonMenu,
  IonToolbar,
  IonTitle,
  IonHeader,
  IonList,
  IonItem,
  IonLabel,
  IonContent,
  IonFooter,
  IonRippleEffect,
  IonToggle,
  IonImg,
  IonText,
  modalController,
  menuController,
} from "@ionic/vue";

import DesignService from "../service/DesignService";
import { contrastSharp, settingsSharp, person } from "ionicons/icons";
import { IonIcon } from "@ionic/vue";
import SettingsModal from "./settings/SettingsModal.vue";
import StorageService from "../service/StorageService";

export default defineComponent({
  name: "MainMenu",
  components: {
    IonMenu,
    IonToolbar,
    IonHeader,
    IonTitle,
    IonList,
    IonItem,
    IonLabel,
    IonToggle,
    IonText,
    IonFooter,
    IonRippleEffect,
    IonImg,
    IonContent,
    IonIcon,
  },
  setup() {
    return {
      DesignService,
      StorageService,
      contrastSharp,
      settingsSharp,
      person,
      visible: ref(false),
    };
  },
  computed: {
    username(): string {
      return StorageService.data.name;
    },
  },
  mounted() {
    const observer = new IntersectionObserver(
      (event) => {
        this.visible = event[0].isIntersecting;
      },
      {
        threshold: 0.9,
      }
    );
    observer.observe((this.$refs.footer as any).$el);
  },
  methods: {
    onToggleDarkmode(event: CustomEvent) {
      DesignService.setDarkMode(event.detail.checked);
    },
    openSettings() {
      menuController.close("main");
      modalController
        .create({
          component: SettingsModal,
        })
        .then((modal) => {
          modal.present();
        });
    },
  },
});
</script>
<style lang="scss" scoped>
.version {
  color: #4d4d4d;
  text-align: center;
}
.watermark {
  display: block;
  margin: auto;
  transition: 800ms;
  &.move-out {
    transform: translateX(100%);
    opacity: 0;
  }
}
.menuIcon {
  height: 14px;
}
</style>
