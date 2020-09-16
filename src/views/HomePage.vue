<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>HOME PAGE</ion-title>
        <ion-buttons slot="end">
          <ion-button @click="router.replace({path:'/login'})">LOGOUT</ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <input type="file" name="file" v-on:change="fileChange($event.target.files)" />
      <div v-if="src">
        <ImageCropper :srcImg="src" />
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
} from "@ionic/vue";
import { defineComponent, ref } from "vue";
import { useRouter } from "vue-router";
import ImageCropper from "./ImageCropper.vue";

export default defineComponent({
  name: "Home",
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    ImageCropper,
  },
  setup() {
    const selectedFile = ref<any>();
    const src = ref<any>();

    const fileChange = (fileList: any) => {
      selectedFile.value = fileList[0];
      if (src.value) window.URL.revokeObjectURL(src.value);
      src.value = window.URL.createObjectURL(fileList[0]);
      console.log("fileChange", src);
    };

    return {
      fileChange,
      src,
      selectedFile,
      router: useRouter(),
    };
  },
});
</script>

<style scoped>
.img-container {
  width: 640px;
  height: 480px;
  float: left;
}
.img-preview {
  width: 200px;
  height: 200px;
  float: left;
  margin-left: 10px;
}
#container {
  text-align: center;

  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>