<template>
  <div class="flex w-full  justify-center flex-col items-center">
    <el-card class="flex justify-center items-center w-[80%] mt-20">
      <h1>Axios Downloader</h1>
      <el-form ref="form" class="w-full">
        <el-form-item label="下载地址">
          <el-input
            placeholder="请输入下载地址"
            v-model="AxDownLoaderOption.url"
            clearable
          >
          </el-input>
        </el-form-item>
        <el-form-item label="存储文件名">
          <el-input
            placeholder="请输入文件名"
            v-model="AxDownLoaderOption.fileName"
            clearable
          >
          </el-input>
        </el-form-item>
      </el-form>
      <div class="flex">
        <button
          @click="downLoad"
          class="py-2 px-4 font-semibold rounded-lg shadow-md text-white bg-green-500 hover:bg-green-700 border-none cursor-pointer"
        >
          download
        </button>
        <!-- <button
          @click="cancelCtr"
          class="ml-3 py-2 px-4 font-semibold rounded-lg shadow-md text-white bg-green-500 hover:bg-green-700 border-none cursor-pointer"
        >
          cancel
        </button> -->
      </div>
      <p>共 {{ bytesToSize(AxDownLoaderOption.fileSize) }}</p>
      <p>下载进度 {{ bytesToSize(AxDownLoaderOption.downloadSize) }}</p>
      <div class="w-80">
        <el-progress
          class="el-bg-inner-done"
          :text-inside="true"
          :stroke-width="24"
          :percentage="AxDownLoaderOption.downloadProgress"
        ></el-progress>
      </div>
      <div class="w-[126px] mt-10">
        <el-progress
          type="circle"
          status="success"
          :percentage="AxDownLoaderOption.downloadProgress"
        ></el-progress>
      </div>
      <div class="w-[160px] mt-10">
        <el-progress
          type="dashboard"
          :stroke-width="14"
          :percentage="AxDownLoaderOption.downloadProgress"
          :width="160"
        >
        </el-progress>
      </div>
    </el-card>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import AxDownLoader from 'axios-downloader'

// 下载进度
const AxDownLoaderOption = ref({
  url: 'https://images.unsplash.com/photo-1663529628961-80aa6ebcd157?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=764&q=80',
  fileName: 'test',
  fileSize: 0,
  downloadSize: 0,
  downloadProgress: 0,
})

//下载
const downLoad = function () {
  AxDownLoader(AxDownLoaderOption.value)
    .then((res) => {
      console.log('downLoad success', res)
      // $message.success('下载成功')
    })
    .catch((err) => {
      console.log('downLoad faild', err)
      // $message.error('下载失败', err)
    })
}
// byte单位换算
const bytesToSize = function (bytes) {
  if (bytes === 0) return '0 B'
  let k = 1024, // or 1024
    sizes = ['B', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB'],
    i = Math.floor(Math.log(bytes) / Math.log(k))
  return (bytes / Math.pow(k, i)).toPrecision(3) + ' ' + sizes[i]
}

// 取消下载
const cancelCtr = function () {
  cancel()
}
</script>

<style scoped>
.el-progress {
  width: 100%;
}
.el-progress__text {
  color: #fff;
  font-size: 14px;
}
.el-progress-bar__outer {
  height: 12px !important;
  border: 1px solid #78335f;
  background-color: transparent;
}
/* 渐变进度条 */
:deep(.el-bg-inner-running .el-progress-bar__inner) {
  background-color: unset;
  background-image: linear-gradient(to right, #3587d8, #6855ff);
}
:deep(.el-bg-inner-error .el-progress-bar__inner) {
  background-image: linear-gradient(to right, #3587d8, #fb3a7e);
}
:deep(.el-bg-inner-done .el-progress-bar__inner) {
  background-image: linear-gradient(to right, #3587d8, #53ff54);
}
</style>
