<template>
  <div v-if="uploading">
    <UploadProgressBar :progress="progress" />
  </div>
  <div v-else class="file-selector">
    <p class="welcome mb-4">
      欢迎来到schematic.cloud！ 请选择您想要上传的schem文件:
    </p>
    <div class="mb-3">
      <input
        tabindex="-1"
        class="form-control form-control-file"
        type="file"
        @change="onChange"
      />
    </div>
    <p class="links mt-4">
      点击这里
      <nuxt-link class="text-decoration-none" to="/download"
        >下载</nuxt-link
      >
      或
      <nuxt-link class="text-decoration-none" to="/delete">删除</nuxt-link
      >schem文件
    </p>
  </div>
</template>

<script setup lang="ts">
import axios from 'axios'

const emits = defineEmits(['success', 'failed'])

const uploading = ref<boolean>(false)
const progress = ref<number>(0.0)

const onChange = async (e: InputEvent) => {
  const files: FileList = (e.target as HTMLFormElement).files

  if (!files || files.length === 0) {
    return
  }
  const file = files[0]

  uploading.value = true

  const formData = new FormData()
  formData.append('schematic', file)

  try {
    const resp = await axios.post(
      `${(await $fetch('/config.json')).api_url}/upload`,
      formData,
      {
        'Content-Type': 'multipart/form-data',
        onUploadProgress: (event) => {
          progress.value = Math.round((event.loaded * 100) / event.total)
        },
      }
    )

    emits('success', {
      download_key: resp.data.download_key,
      delete_key: resp.data.delete_key,
    })
  } catch (err) {
    let status
    if (err.response) {
      status = err.response.status
    }

    let error
    switch (status) {
      case 400:
        error =
          'The file you uploaded was not valid NBT, and so it has been rejected. Please upload a valid NBT file.'
        break
      case 500:
        error =
          'The file could not be saved at the server level, and so it your upload was rejected. Please try again, or report this issue to the developer.'
        break
      case undefined:
      default:
        error =
          'An unknown error occurred while trying to upload your file. Please try again, or report this issue to the developer.'
        break
    }

    emits('failed', error)
  }

  uploading.value = false
}
</script>
