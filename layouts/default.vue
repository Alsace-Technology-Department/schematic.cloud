<template>
  <Html>
    <Head>
      <Title>工业园 schematic cloud</Title>
      <Meta charset="UTF-8"></Meta>
      <Meta
        name="viewport"
        content="width=device-width, initial-scale=1"
      ></Meta>
      <Meta
        name="description"
        hid="description"
        content="上传schem到工业园"
      ></Meta>
      <Link rel="icon" type="image/x-icon" href="https://ns.alsace.team/service/images/logo/favicon.ico"></Link>
    </Head>
    <Body>
      <div class="container">
        <div class="row pt-5">
          <div class="col-8 mx-auto">
            <div class="card bg-dark bg-opacity-90 text-light">
              <div class="card-body">
                <nuxt-link to="/">
                  <img
                    class="d-block my-0 mx-auto"
                    src="~/assets/img/logo.svg"
                    alt="schematic.cloud"
                  />
                </nuxt-link>
                <nuxt-page></nuxt-page>
              </div>
              <div class="card-footer">
                <p class="m-0 text-center">
                  schematic.cloud version {{ version }}. Made with
                  <span class="text-danger">&hearts;</span> by
                  <a
                    class="text-decoration-none"
                    href="https://github.com/IntellectualSites/schematic.cloud"
                    target="_blank"
                  >
                    IntellectualSites
                  </a> & 
                  <a
                    class="text-decoration-none"
                    href="https://www.alsace.team"
                    target="_blank"
                  >
                    AlsaceTeam
                  </a>
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </Body>
  </Html>
</template>

<script setup lang="ts">
import { Config } from '~/types'

const version = ref<string>('loading')

onMounted(async () => {
  version.value = (
    await $fetch((await $fetch<Config>('/config.json')).api_url)
  ).version
})
</script>

<style>
body {
  background-image: url('https://ns.alsace.team/service/images/img/arks_max.JPG');
  background-position: center;
  background-size: cover;
  min-height: 100vh;
}
</style>
