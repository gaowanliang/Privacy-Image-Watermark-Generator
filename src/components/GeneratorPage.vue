<template>
  <n-config-provider :theme-overrides="themeOverrides" :locale="locale">
    <n-layout embedded>
      <PageHead :lang="lang" :lang-change="setLang" @langChange="setLang" />
      <div class="content-box">
        <div class="content">
          <GenerateModule :lang="lang" />
        </div>
      </div>
    </n-layout>
  </n-config-provider>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import { NLayout, NConfigProvider, type GlobalThemeOverrides, zhCN } from 'naive-ui'
import PageHead from './PageHead.vue'
import GenerateModule from './GenerateModule.vue'

export default defineComponent({
  name: 'GeneratorPage',
  components: {
    PageHead,
    NLayout,
    NConfigProvider,
    GenerateModule
  },
  setup: () => {
    const lang = ref<'en-US' | 'zh-CN'>('en-US')
    const locale = ref<undefined | typeof zhCN>()

    const setLang = (value: 'en-US' | 'zh-CN') => {
      lang.value = value
      locale.value = value === 'en-US' ? undefined : zhCN
    }
    const themeOverrides: GlobalThemeOverrides = {
      common: {
        fontSize: '15px',
        fontSizeMedium: '15px',
        fontSizeLarge: '16px'
      },
      Card: {
        titleFontSizeMedium: '20px'
      },
      Form: {
        labelFontSizeTopLarge: '15px'
      }
    }

    return {
      lang,
      locale,
      themeOverrides,
      setLang
    }
  }
})
</script>

<style scoped>
.content-box {
  margin: auto;
  width: var(--content-width);
  max-width: var(--content-max-width);
}

.content {
  margin-top: 24px;
}
</style>
