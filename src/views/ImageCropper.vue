<template>
  <div>
    <button @click.prevent="cropper.zoom(0.2)">Zoom In</button>
    <button  @click.prevent="cropper.zoom(-0.2)">Zoom Out</button>

    <div class="img-container">
      <img ref="image" :src="sourceImg" crossorigin class="img-container" />
    </div>
    <img :src="destination" class="img-preview" />
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref, watch } from "vue";
import Cropper from "cropperjs";
import "cropperjs/dist/cropper.css";
export default defineComponent({
  name: "ImageCropper",
  props: {
    srcImg: String,
  },
  setup(props: any) {
    const sourceImg = ref<any>(props.srcImg);
    const cropper = ref<any>(null);
    let canvas: any = null;
    const image = ref<any>(null);
    const destination = ref<any>();

    const setCropper = () => {
      //   sourceImg.value = props.srcImg;
      cropper.value = new Cropper(image.value, {
        zoomable: true,
        scalable: false,
        aspectRatio: 1,
        crop: () => {
          canvas = cropper.value.getCroppedCanvas({
            imageSmoothingEnabled: false,
            imageSmoothingQuality: "high",
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
      debugger;
      if (!cropper.value) {
        setCropper();
      }
    });

    return {
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
</style>