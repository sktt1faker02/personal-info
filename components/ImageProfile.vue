<template>
  <div class="flex">
    <div class="card bg-white rounded-xl text-center flex flex-col items-center gap-4 mt-4">
      <img ref="profilePic" class="w-[100px] h-[100px] rounded-full border border-primary object-cover" :src="defaultUserPicture" alt="default picture" />
      <label class="block w-[200px] bg-slate-200 text-primary font-bold p-2 my-2 mx-auto rounded cursor-pointer" for="input-file">Upload Profile Image</label>
      <input @change="handleInputFile" class="hidden" type="file" accept="image/jpeg, image/png, image/jpg" ref="inputFile" id="input-file" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";

import defaultUserPicture from "../assets/img/default-profile.jpg";

const props = defineProps<{
  isReset?: boolean;
  updateImg: {
    isPopulate: boolean;
    imgSrc: string;
  };
}>();

const profilePic = ref<HTMLImageElement>();
const inputFile = ref<HTMLInputElement>();

const emit = defineEmits(["imageSrc", "imageEl"]);

onMounted(() => {
  emit("imageEl", profilePic.value);
});

const handleInputFile = () => {
  if (inputFile.value?.files && profilePic.value) {
    const imgSrc = URL.createObjectURL(inputFile.value.files[0]);
    profilePic.value.src = URL.createObjectURL(inputFile.value.files[0]);

    emit("imageSrc", imgSrc);
  }
};

// watch(
//   () => props.isReset,
//   (newValue) => {
//     console.log(newValue);
//   }
// );

watch(
  () => props.updateImg.isPopulate,
  (newValue) => {
    // profilePic.value.src = props.updateImg.imgSrc;
    if (newValue && profilePic.value) {
      profilePic.value.src = props.updateImg.imgSrc;
    } else if (profilePic.value) {
      profilePic.value.src = defaultUserPicture;
    }
  }
);
// watch(
//   () => props.updateImg.isPopulate,
//   (newValue) => {
//     if (newValue && profilePic.value) {
//       profilePic.value.src = props.updateImg.imgSrc;
//     } else if (profilePic.value) {
//       profilePic.value.src = defaultUserPicture;
//     }
//   }
// );

// console.log(props.isReset, props.updateImg.isPopulate);

// console.log(props.updateImg);
</script>

<style scoped></style>
