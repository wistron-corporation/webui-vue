<template>
  <b-container fluid="xl">
    <page-title />
    <alerts-server-power
      v-if="isServerPowerOffRequired"
      :is-server-off="isServerOff"
    />

    <!-- Firmware cards -->
    <b-row>
      <!-- <page-section: section-title="$t('pageFirmware.sectionTitleUpdateFirmware')"> -->
      <page-section :section-title="$t('BMC Capsule Image')">
        <b-col>
          <!-- BMC Capsule -->
          <pfr-cards :is-page-disabled="isPageDisabled" />
        </b-col>

        <!-- Update firmware-->
        <b-col>
          <!-- Update form -->
          <form-update
            :is-server-off="isServerOff"
            :is-page-disabled="isPageDisabled"
          />
        </b-col>
      </page-section>
    </b-row>
  </b-container>
</template>

<script>
import AlertsServerPower from './FirmwareAlertServerPower';
import PfrCards from './FirmwarePFRCards';
import FormUpdate from './FirmwareFormUpdate';
import PageSection from '@/components/Global/PageSection';
import PageTitle from '@/components/Global/PageTitle';

import LoadingBarMixin, { loading } from '@/components/Mixins/LoadingBarMixin';

export default {
  name: 'PFRCapsule',
  components: {
    AlertsServerPower,
    PfrCards,
    FormUpdate,
    PageSection,
    PageTitle,
  },
  mixins: [LoadingBarMixin],
  beforeRouteLeave(to, from, next) {
    this.hideLoader();
    next();
  },
  data() {
    return {
      loading,
      isServerPowerOffRequired:
        process.env.VUE_APP_SERVER_OFF_REQUIRED === 'true',
    };
  },
  computed: {
    serverStatus() {
      return this.$store.getters['global/serverStatus'];
    },
    isServerOff() {
      return this.serverStatus === 'off' ? true : false;
    },
    // isSingleFileUploadEnabled() {
    //   return this.$store.getters['PFR/isSingleFileUploadEnabled'];
    // },
    isPageDisabled() {
      if (this.isServerPowerOffRequired) {
        return !this.isServerOff || this.loading || this.isOperationInProgress;
      }
      return this.loading || this.isOperationInProgress;
    },
  },
  created() {
    this.startLoader();
    this.$store
      .dispatch('PFR/getFirmwareInformation')
      .finally(() => this.endLoader());
  },
};
</script>
