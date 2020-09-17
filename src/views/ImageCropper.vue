<template>
  <div>
    <ion-button size="small" @click.prevent="cropper.zoom(0.2)">
      <ion-icon src="./assets/icons/svg/003-zoom-in.svg" slot="icon-only" size="small" />
    </ion-button>
    <ion-button size="small" @click.prevent="cropper.zoom(-0.2)">
      <ion-icon src="./assets/icons/svg/004-zoom-out.svg" slot="icon-only" size="small" fill="white" />
    </ion-button>
    <ion-button size="small" @click.prevent="cropper.rotate(-90)">
      <ion-icon src="./assets/icons/svg/001-rotate-left.svg" slot="icon-only" size="small" />
    </ion-button>
    <ion-button size="small" @click.prevent="cropper.rotate(90)">
      <ion-icon src="./assets/icons/svg/002-rotate-right.svg" slot="icon-only" size="small" />
    </ion-button>

    <div class="img-container">
      <img ref="image" :src="sourceImg" crossorigin class="img-container" />
    </div>
    <div>
      <img :src="destination" class="img-preview" />
    </div>
    <ion-button size="small" @click="getCroppedBlob">GET BLOB</ion-button>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref, watch } from "vue";
import { IonIcon } from "@ionic/vue";
import Cropper from "cropperjs";
import "cropperjs/dist/cropper.css";
export default defineComponent({
  name: "ImageCropper",
  components: {
    IonIcon,
  },
  props: {
    srcImg: String,
  },
  setup(props: any) {
    const sourceImg = ref<any>(props.srcImg);
    const cropper = ref<any>(null);
    let canvas: any = null;
    const image = ref<any>(null);
    const destination = ref<any>();

    const getCroppedBlob = () => {
      const canvas = cropper.value.getCroppedCanvas();
      canvas.toBlob((blob: any) => {
        console.log(blob);
        debugger;
      });
    };

    const setCropper = () => {
      //   sourceImg.value = props.srcImg;
      cropper.value = new Cropper(image.value, {
        zoomable: true,
        scalable: false,
        aspectRatio: 1,
        crop: () => {
          canvas = cropper.value.getCroppedCanvas({
            // imageSmoothingEnabled: false,
            // imageSmoothingQuality: "high",
          });
          destination.value = canvas.toDataURL("image/png");
        },
      });
      console.log(props.srcImg);
    };

    watch(
      () => props.srcImg,
      (cur: string) => {
        cropper.value.clear();
        cropper.value.replace(cur);
      }
    );

    onMounted(() => {
      if (cropper.value) {
        cropper.value.destroy();
      }
      setCropper();
    });

    return {
      getCroppedBlob,
      sourceImg,
      cropper,
      image,
      destination,
    };
  },
});
</script>

<style scoped>
.img-container {
  width: 100%;
  height: 320px;
}
.img-preview {
  width: 200px;
  height: 200px;
  padding: 10px;
}
</style>