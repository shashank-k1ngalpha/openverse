<template>
  <div>
    <h3 id="copy-license-title" class="description-bold md:heading-6 mb-4">
      {{ $t("media-details.reuse.copy-license.title") }}
    </h3>

    <VTabs label="#copy-license-title" :selected-id="tabs[0]">
      <template #tabs>
        <VTab v-for="tab in tabs" :id="tab" :key="tab">
          {{ $t(`media-details.reuse.copy-license.${tab}`) }}
        </VTab>
      </template>
      <VLicenseTabPanel
        :tab="tabs[0]"
        :media-id="media.id"
        :media-type="media.frontendMediaType"
      >
        <!-- Disable reason: We control the attribution HTML generation so this is safe and will not lead to XSS attacks -->
        <!-- eslint-disable vue/no-v-html -->
        <div v-html="getAttributionMarkup({ includeIcons: false })" />
        <!-- eslint-enable vue/no-v-html -->
      </VLicenseTabPanel>
      <VLicenseTabPanel
        :tab="tabs[1]"
        :media-id="media.id"
        :media-type="media.frontendMediaType"
      >
        <p id="attribution-html" class="break-all font-mono" dir="ltr">
          {{ getAttributionMarkup() }}
        </p>
      </VLicenseTabPanel>
      <VLicenseTabPanel
        :tab="tabs[2]"
        :media-id="media.id"
        :media-type="media.frontendMediaType"
      >
        {{ getAttributionMarkup({ isPlaintext: true }) }}
      </VLicenseTabPanel>
    </VTabs>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue"

import { AttributionOptions, getAttribution } from "~/utils/attribution-html"
import type { Media } from "~/types/media"
import { useI18n } from "~/composables/use-i18n"

import VTabs from "~/components/VTabs/VTabs.vue"
import VTab from "~/components/VTabs/VTab.vue"
import VLicenseTabPanel from "~/components/VMediaInfo/VLicenseTabPanel.vue"

const tabs = ["rich", "html", "plain"] as const

export default defineComponent({
  name: "VCopyLicense",
  components: { VTabs, VTab, VLicenseTabPanel },
  props: {
    media: {
      type: Object as PropType<Media>,
      required: true,
    },
  },
  setup(props) {
    const i18n = useI18n()
    const getAttributionMarkup = (options?: AttributionOptions) =>
      getAttribution(props.media, i18n, options)
    return {
      tabs,

      getAttributionMarkup,
    }
  },
})
</script>
