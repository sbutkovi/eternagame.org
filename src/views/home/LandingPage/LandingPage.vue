<template>
  <div class="page">

    <HeroSection />

    <!-- Covid-19 Promo (TODO: localize?)-->
    <div class="container bs-docs-section mt-5">
      <div class="media row">
        <img
          src="https://eternagame.org/puzzle-progression/badges/badge_openvaccine_100.png"
          class="align-self-center mr-3 mb-3 col-sm-12 col-lg-4"
          alt="Fight COVID-19 with Eterna"
        />
        <div class="media-body p-4">
          <h3 class="mt-0" style="font-weight: bold;">OpenVaccine: Eterna takes on COVID-19</h3>
          <hr style="border-top: 1px solid rgba(255, 255, 255, 0.4)" />
          <p style="font-size: 1.2rem;">
            With the onset of the COVID-19 pandemic in 2020, the Eterna project harnessed online
            gamers toward a solution: the OpenVaccine challenge. Their mission? To develop mRNA
            vaccines stable enough to be deployed to everyone in the world and not just a privileged
            few. Check out the <router-link to="/challenges/10845741">challenge page</router-link>
            for a description of our accomplishments and publications, including special sequences
            for fighting the Delta and Omicron variants.
          </p>
          <p class="mb-0"></p>
        </div>
      </div>
    </div>
    <!-- End Promo -->

    <!-- 10 Year Anniversary Promo (TODO: localize?)-->
    <div class="container bs-docs-section mt-5">
      <div class="media row">
        <div class="media-body p-4">
          <h3 class="mt-0" style="font-weight: bold;">10 Years of Discovery</h3>
          <hr style="border-top: 1px solid rgba(255, 255, 255, 0.4)" />
          <p style="font-size: 1.2rem;">
            Eterna has led over 10 years of citizen science. Together we've
            synthesized thousands of player-designed molecules, published over 20 academic papers
            tackling fundamental RNA design questions, and pioneered the open development of RNA
            medicines. We will continue to roll out new features, game
            mechanics, and lab challenges as we expand the future of Eterna.
          </p>
          <p class="mb-0"></p>
        </div>
        <img
          src="~@/assets/front-page/img/eterna_10yr-transp.png"
          class="align-self-center mr-3 mb-3 col-sm-12 col-lg-4"
          alt="Eterna 10th anniversary"
        />
      </div>
    </div>
    <!-- End Promo -->

    <!-- NOVA Promo (TODO: localize?)-->
    <div class="container bs-docs-section mt-5">
      <div class="media row">
        <img
          src="/puzzle-progression/badges/badge_nova_large.png"
          class="align-self-center mr-3 mb-3 col-sm-12 col-lg-3"
          alt="Learn about RNA with NOVA" border="0"
        />
        <div class="media-body p-4">
          <h3 class="mt-0" style="font-weight: bold;">Learn more about RNA</h3>
          <hr style="border-top: 1px solid rgba(255, 255, 255, 0.4)" />
          <p style="font-size: 1.2rem;">
            Eterna is proud to partner with NOVA Labs to help accelerate the fight against the
            coronavirus. Learn about RNA vaccines and viruses from
            <a href="https://www.pbs.org/wgbh/nova/video/decoding-covid-19/" target="_blank">NOVA</a>.
            Check out the <a href="https://www.pbs.org/wgbh/nova/labs/lab/rna/" target="_blank">RNA VirtuaLab</a>
            to get a crash course on what you can do to help. Then, unlock lab access to join our lab challenges!
          </p>
          <p class="mb-0"></p>
        </div>
      </div>
    </div>
    <!-- End Promo -->

    <DocsSection />
  </div>
</template>

<script lang="ts">
  import { Component, Vue, Mixins } from 'vue-property-decorator';
  import { RouteCallback, Route } from 'vue-router';
  import { AxiosInstance } from 'axios';
  import DocsSection from './components/DocsSection.vue';
  import HeroSection from './components/HeroSection.vue';

  @Component({
    components: {
      DocsSection,
      HeroSection,
    },
  })
  export default class LandingPage extends Vue {
    get loggedIn() {
      return this.$vxm.user.loggedIn;
    }
  }
</script>

<style lang="scss" scoped>
  .page {
    margin-top: 120px;
  }

  .eternacon-banner {
    width: 100%;
    max-width: 1200px;
  }
</style>
