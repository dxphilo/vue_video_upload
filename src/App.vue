<script lang="ts">
import { ref } from "vue";
export default {
  setup() {
    const selectedFile = ref();
    const videoPreviewURL = ref();

    function onFileSelected(event: Event) {
      if (!event || !event.target) return;

      const target = event.target as HTMLInputElement;
      if (!target.files || target.files.length === 0) {
        alert("No file selected, please choose a file to upload.");
        return;
      }

      const selectedFileType = target.files[0].type;
      if (!selectedFileType.startsWith("video/")) {
        alert("Invalid file type. Please select a video file.");
        target.value = "";
        return;
      }

      selectedFile.value = target.files[0];
      console.log("Selected file:", selectedFile.value);

      createVideoPreview();
    }

    function createVideoPreview() {
      videoPreviewURL.value = URL.createObjectURL(selectedFile.value);
    }
    async function uploadVideo() {
      const uploadVideoUrl = "https://example.com/upload/video";

      const formData = new FormData();

      formData.append("video", selectedFile.value);
      try {
        const response = await fetch(uploadVideoUrl, {
          method: "POST",
          body: formData,
        });

        if (response.ok) {
          alert("Video uploaded successfully");
        }
      } catch (error) {
        alert("Failed to upload video");
      }
    }

    return {
      onFileSelected,
      selectedFile,
      videoPreviewURL,
      uploadVideo,
    };
  },
};
</script>

<template>
  <div>
    <h2 class="title">Vue.js Video Upload</h2>

    <video
      v-if="videoPreviewURL"
      id="my-video"
      class="video-js"
      controls
      preload="auto"
      width="640"
      height="264"
      data-setup="{}"
    >
      <source :src="videoPreviewURL" type="video/mp4" />
      <p class="vjs-no-js">
        To view this video please enable JavaScript, and consider upgrading to a
        web browser that
        <a href="https://videojs.com/html5-video-support/" target="_blank"
          >supports HTML5 video</a
        >
      </p>
    </video>

    <main>
      <form enctype="multipart/form-data" class="form">
        <label for="video" class="label">Select a video:</label>
        <input
          type="file"
          id="video"
          name="video"
          accept="video/*"
          class="input"
          required
          @change="onFileSelected"
        />
        <button type="submit" class="button" @click.prevent="uploadVideo()">
          Upload
        </button>
      </form>
    </main>
  </div>
</template>

<style scoped>
.title {
  text-align: center;
  padding: 20px 0;
}

main {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 600px;
  margin: 0 auto;
}

.form {
  border: 2px solid #f4f4f4;
  padding: 20px;
  border-radius: 8px;
}

.label {
  font-size: 18px;
  margin-bottom: 8px;
  display: block;
}

.input {
  padding: 10px;
  margin-bottom: 16px;
  width: 100%;
  box-sizing: border-box;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.button {
  padding: 10px 20px;
  background-color: #3498db;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.button:hover {
  background-color: #2980b9;
}
.video {
  display: block;
  margin: 30px auto;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
