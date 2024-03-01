<template>
  <header class="p-4">
    <h1 class="text-3xl text-center font-bold text-primary mb-4">Personal Information</h1>
    <p class="text-tertiary font-medium">Please provide the required information below.</p>
  </header>

  <main class="px-4">
    <section class="personal-form">
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
          </div>
          <!-- <p>{{ formValues.name }}</p> -->
          <!-- <input type="text" v-model="formValues.name" /> -->
          <button class="py-2 bg-primary text-white rounded-md">Submit</button>
        </form>
      </Card>
    </section>
  </main>
</template>

<script setup lang="ts">
import { ref } from "vue";

const formValues = ref({
  name: "",
  email: "",
  educationLevel: "",
  gender: "",
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

const submitForm = () => {
  console.log("Form values", formValues.value);
};
</script>
