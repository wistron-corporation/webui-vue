<template>
  <div>
    <page-section :section-title="sectionTitle">
      <b-card-group deck>
        <!-- Running image -->
        <b-card>
          <template #header>
            <p class="font-weight-bold m-0">
              {{ $t('pageFirmware.cardTitleRunning') }}
            </p>
          </template>
          <dl class="mb-0">
            <dt>{{ $t('pageFirmware.cardBodyVersion') }}</dt>
            <dd class="mb-0">{{ runningVersion }}</dd>
          </dl>
        </b-card>
      </b-card-group>
    </page-section>
  </div>
</template>

<script>
//import PageSection from '@/components/Global/PageSection';
import LoadingBarMixin, { loading } from '@/components/Mixins/LoadingBarMixin';
import BVToastMixin from '@/components/Mixins/BVToastMixin';

export default {
  //components: { PageSection },
  mixins: [BVToastMixin, LoadingBarMixin],
  props: {
    isPageDisabled: {
      required: true,
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      loading,
      switchToBackupImageDisabled:
        process.env.VUE_APP_SWITCH_TO_BACKUP_IMAGE_DISABLED === 'true',
    };
  },
  computed: {
    // isSingleFileUploadEnabled() {
    //   return this.$store.getters['firmware/isSingleFileUploadEnabled'];
    // },
    // sectionTitle() {
    //   if (this.isSingleFileUploadEnabled) {
    //     return this.$t('pageFirmware.sectionTitleBmcCardsCombined');
    //   }
    //   return this.$t('pageFirmware.sectionTitleBmcCards');
    // },
    running() {
      return this.$store.getters['PFR/activeBmcFirmware'];
    },
    runningVersion() {
      return this.running?.version || '--';
    },
  },
};
</script>
