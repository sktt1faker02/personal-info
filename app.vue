<template>
  <header class="p-4">
    <h1 class="text-3xl text-center font-bold text-primary mb-4">Personal Information</h1>
    <p class="text-tertiary font-medium">Please provide the required information below.</p>
  </header>

  <main class="px-4">
    <section class="personal-form grid md:grid-cols-2 gap-4">
      <Card shadow="shadow-md" radius="rounded-md">
        <form class="flex flex-col gap-4" @submit.prevent="submitForm">
          <div class="input-field flex flex-col gap-2">
            <TextInput label="Name" v-model="formValues.name" id="name" placeholder="Name" :required="true" type="text" />
            <TextInput label="Email Address" v-model="formValues.email" id="email" placeholder="Email" :required="true" type="email" />

            <Dropdown id="educationLevel" v-model="formValues.educationLevel" label="Education Level" :options="educationLevelChoices" />

            <div class="gender">
              <h5>Select Gender</h5>
              <Radio label="Male" id="male" value="male" v-model="formValues.gender" />
              <Radio label="Female" id="female" value="female" v-model="formValues.gender" />
            </div>

            <ImageProfile @imageEl="(e:HTMLImageElement) => imgElement = e " @imageSrc="(url: string) => formValues.imgSrc = url" :isReset="isResetUserImg" :updateImg="updateImg" />
          </div>

          <!-- <input type="text" v-model="formValues.name" /> -->
          <button class="py-2 bg-primary text-white rounded-md">{{ isUpdate ? "Update" : "Submit" }}</button>
        </form>
      </Card>

      <Card shadow="shadow-lg" radius="rounded-lg">
        <aside>
          <h2 class="text-primary text-xl font-bold mb-2">List of Users</h2>
          <div class="border-b border-primary mb-2"></div>

          <template v-if="persons?.length">
            <div v-for="person of persons" :key="person.id">
              <ul class="border border-primary p-4">
                <div class="flex justify-between gap-2">
                  <div>
                    <li>{{ person.name }}</li>
                    <li>{{ person.email }}</li>
                    <li>{{ educationLevelChoices.find((level) => level.value === person.educationLevel)?.label }}</li>
                    <li>{{ person.gender }}</li>
                  </div>
                  <div>
                    <img class="w-[50px] h-[50px] rounded-full object-cover" :src="person.imgSrc" alt="caching image" />
                  </div>
                </div>
                <div class="control flex gap-4 mt-4">
                  <button @click="updateUser(person)" class="bg-white text-primary px-2 border border-primary">Edit</button>
                  <button @click="removeUser(person.id)" class="bg-white text-red-500 px-2 border border-red-500">Delete</button>
                </div>
              </ul>
            </div>
          </template>
        </aside>
      </Card>
    </section>
  </main>
</template>

<script setup lang="ts">
import { ref } from "vue";
import type { PersonsData } from "./types/types";
import defaultUserPicture from "./assets/img/default-profile.jpg";

const BASE_API = "http://localhost:5000/persons";

const { data: persons, refresh } = await useFetch<PersonsData[]>(BASE_API);

const isResetUserImg = ref(false);
const updateImg = ref({
  isPopulate: false,
  imgSrc: "",
});

const isUpdate = ref(false);

const formValues = ref({
  name: "",
  email: "",
  educationLevel: "",
  gender: "",
  imgSrc: "",
});

const educationLevelChoices = [
  {
    id: 1,
    value: "college",
    label: "College/University",
  },
  {
    id: 2,
    value: "highschool",
    label: "High School",
  },
  {
    id: 3,
    value: "elementary",
    label: "Elementary",
  },
];

// const getImg = (url: string) => {
//   console.log(url);
// };

// INSERT

const resetForm = () => {
  formValues.value.name = "";
  formValues.value.email = "";
  formValues.value.educationLevel = "";
  formValues.value.gender = "";
  formValues.value.imgSrc = "";
  isResetUserImg.value = true;
  updateImg.value.isPopulate = false;
};
const imgElement = ref<HTMLImageElement>();

// const imgElement = new Image();

const isUpdateId = ref<string | number>("");

const submitForm = async () => {
  console.log(imgElement);
  if (imgElement.value) {
    imgElement.value.src = defaultUserPicture;
  }

  if (!formValues.value.imgSrc) formValues.value.imgSrc = defaultUserPicture;

  // if(isUpdate.value)
  if (!isUpdate.value) {
    const response = await $fetch(BASE_API, {
      method: "POST",
      headers: {
        "Content-type": "application/json",
      },
      body: formValues.value,
    });
  } else {
    try {
      const response = await $fetch(`${BASE_API}/${isUpdateId.value}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: formValues.value,
      });

      // console.log({ response });
    } catch (err) {
      console.log(err, "Error updating user");
    }
  }
  isUpdate.value = false;

  resetForm();

  refresh();
};

// UPDATE

const updateUser = (person: PersonsData) => {
  isUpdate.value = true;

  const { id, name, email, gender, educationLevel, imgSrc } = person;

  formValues.value.name = name;
  formValues.value.email = email;
  formValues.value.gender = gender;
  formValues.value.educationLevel = educationLevel;
  formValues.value.imgSrc = imgSrc;
  updateImg.value.isPopulate = true;
  updateImg.value.imgSrc = imgSrc;

  isUpdateId.value = id;
};

// DELETE
const removeUser = async (id: string | number) => {
  if (confirm("Are you sure?")) {
    // console.log(id);
    try {
      const response = await $fetch(`${BASE_API}/${id}`, {
        method: "DELETE",
      });
      refresh();

      // console.log({ response });
    } catch (err) {
      console.log(err, "Error deleting user");
    }
  }
};
</script>
