<template>
  <div>
    <div v-if="result.error" class="text">
      <b class="d-block py-4">{{ result.error }}</b>
    </div>
    <div v-else>
      <div
        class="modal fade bg-opacity-50 bg-dark show"
        :class="{
          'd-block': modal,
        }"
        tabindex="-1"
      >
        <div class="modal-dialog">
          <div class="modal-content bg-dark">
            <div class="modal-header">
              <h1 class="modal-title fs-5">Really delete?</h1>
              <button
                type="button"
                class="btn-close btn-close-white"
                data-bs-dismiss="modal"
                aria-label="Close"
                @click="toggleDeleteModal"
              ></button>
            </div>
            <div class="modal-body">
              您确定要删除该schem文件吗？如果您这样做，您的文件将立即从我们的服务器上删除，且无法撤销。
            </div>
            <div class="modal-footer">
              <button
                type="button"
                class="btn btn-info"
                data-bs-dismiss="modal"
                @click="toggleDeleteModal"
              >
                Cancel
              </button>
              <button
                type="button"
                class="btn btn-danger"
                @click="handleDeleteConfirm"
              >
                Confirm
              </button>
            </div>
          </div>
        </div>
      </div>
      <div class="text">
        <p>
          请在服务器执行以下命令来加载schem。
        </p>
      </div>
      <UploadCopyableText
        name="在游戏中使用"
        :value="downloadUrl(result.download_key!)"
        :is-url="true"
        url-button-txt="Download"
        url-button-variant="success"
      />
      <div class="row">
        <div class="col-6">
          <button
            class="btn btn-danger d-block w-100"
            @click="toggleDeleteModal"
          >
            删除
          </button>
        </div>

        <div class="col-6">
          <button
            class="btn btn-success d-block w-100"
            @click="handleDownloadClick"
          >
            下载
          </button>
        </div>
      </div>
    </div>
    <button
      class="btn btn-secondary d-block w-100 mt-3"
      @click="emits('reset')"
    >
      上传其他文件
    </button>
  </div>
</template>

<script setup lang="ts">
import { PropType } from 'vue'
import { Config } from '~/types'

const modal = ref(false)
const emits = defineEmits(['reset'])

const props = defineProps({
  result: {
    type: Object as PropType<{
      download_key: string | undefined
      delete_key: string | undefined
      error: string | undefined
    }>,
    required: true,
  },
})

const downloadUrl = async (key: string) => {
  return `//schem load url:${key}`
}

const handleDownloadClick = async () => {
  await useRouter().push(`/download/${props.result!.download_key}`)
}

const toggleDeleteModal = () => {
  modal.value = !modal.value
}

const handleDeleteConfirm = async () => {
  await useRouter().push(`/delete/${props.result!.delete_key}`)
}
</script>
