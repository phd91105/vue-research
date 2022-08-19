<script lang="ts">
import { computed, ref } from "vue";
import axios from "axios";

export default {
  name: "App",
  setup() {
    const inputLink = ref("");
    const finalLink = ref("");
    const password = ref("");
    const isLoading = ref(false);

    const buttonContent = computed(() => {
      return isLoading.value
        ? `<span class="spinner-border spinner-border-sm ml-4 mr-4" role="status" aria-hidden="true"></span>`
        : "Get link";
    });

    const handleSubmitDisable = computed(() => {
      return inputLink.value == "" || isLoading.value ? true : false;
    });
    const handleButtonDisable = computed(() => {
      return finalLink.value == "" ? true : false;
    });

    const submitEvent = async () => {
      isLoading.value = true;
      const submitUrl = `https://fshare.phamduy.me/generate?url=${encodeURI(
        inputLink.value
      )}&password=${password.value}`;

      const data = await axios.get(submitUrl);
      isLoading.value = false;

      finalLink.value = data.data.location;
    };

    return {
      inputLink,
      finalLink,
      password,
      handleSubmitDisable,
      handleButtonDisable,
      submitEvent,
      buttonContent,
    };
  },
};
</script>

<template>
  <main>
    <b-container id="mainForm" class="mt-5">
      <img
        height="50"
        width="143"
        class="mb-3"
        src="https://fshare.phamduy.me/assets/images/fshare-logo-17072020.svg"
        alt="Fshare"
      />
      <b-form-group>
        <label class="form-label"
          >Fshare Link <span class="text-danger">*</span></label
        >
        <input
          id="original"
          type="text"
          class="form-control"
          placeholder="https://www.fshare.vn/file/XXXXX"
          v-model="inputLink"
          required
        />
      </b-form-group>
      <b-form-group>
        <label class="form-label">File password</label>
        <input
          id="password"
          type="text"
          class="form-control"
          placeholder="Password"
          v-model="password"
        />
      </b-form-group>
      <b-form-group>
        <label id="fileName" class="form-label">Generated Link</label>
        <input
          id="generated"
          type="text"
          class="form-control"
          disabled
          v-model="finalLink"
        />
      </b-form-group>
      <b-form-group class="mt-3">
        <b-button
          :disabled="handleSubmitDisable"
          @click="submitEvent"
          id="create"
          variant="primary"
          class="me-2"
          style="width: 90px"
          ><span v-html="buttonContent"></span
        ></b-button>
        <b-button
          id="copy"
          variant="success"
          class="me-2"
          :disabled="handleButtonDisable"
          >Copy</b-button
        >
        <b-button
          id="download"
          variant="danger"
          class="me-2"
          :disabled="handleButtonDisable"
          >Download</b-button
        >
        <b-button
          id="openInVLC"
          variant="warning"
          class="me-2"
          :disabled="handleButtonDisable"
          >Open in VLC</b-button
        >
      </b-form-group>
    </b-container>
  </main>
</template>

<style scoped></style>
