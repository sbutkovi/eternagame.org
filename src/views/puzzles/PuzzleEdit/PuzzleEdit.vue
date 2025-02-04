<template>

  <EternaPage v-if="fetchState.firstFetchComplete && puzzle">

    <b-form-input v-model="puzzTitle" no-resize size="lg"></b-form-input>
    <div class="page-content">
      <h2>About the Puzzle</h2>
      <div class="d-flex flex-wrap justify-content-between" xs="12" sm="8">
        <div style="text-align:center" class="order-sm-2 image-col">
          <div class="puzzle-image">
            <img v-if="imageURL" :src="imageURL" />
          </div>
          <b-button
            type="submit"
            variant="primary"
            class="submit-button"
            @click="submit"
            :disabled="!puzzBody || !puzzTitle || !access"
          >
          {{ $t('edit-submit') }}
          </b-button>
        </div>

        <div class="order-sm-1 description-col">
          <hr class="top-border d-sm-none" />
          <b-textarea v-model="puzzBody" rows="12" max-rows="12" no-resize></b-textarea>
          <div/>
        </div>
      </div>
    </div>


    <template #sidebar="{ isInSidebar }">
      <SidebarPanel
        :isInSidebar="isInSidebar"
        :header="$t('puzzle-view:side-bar-header')"
        headerIcon="@/assets/info.svg"
      >
        <template #header-icon>
          <img src="@/assets/info.svg" />
        </template>
        <ul style="padding: 0; list-style-type:none" v-if="puzzle">
          <li v-if="madeByPlayer">
            <img :src="avatar" class="icon" />{{ puzzle.username }}
          </li>
          <li v-if="puzzle.folder">
            <img src="@/assets/chemical_bond.svg" class="icon" />{{ puzzle.folder }}
          </li>
          <li v-if="puzzle.reward">
            <img src="@/assets/dollar.svg" class="icon" />{{ puzzle.reward }}
          </li>
          <li>
            <img src="@/assets/people.svg" class="icon" />
            {{ puzzle['num-cleared'] ? puzzle['num-cleared'] : 0 }}
          </li>
          <li v-if="puzzle.created">
            <img src="@/assets/calendar.svg" class="icon" />{{ puzzle.created }}
          </li>
          <li v-if="clearedThisPuzzle">
            <img src="@/assets/noun_check.svg" class="icon" />Cleared
          </li>
          <li>
          <b-button
            type="submit"
            variant="primary"
            class="submit-button"
            :href="`${tutorialRoute}${nid}`"
          >
          {{ $t('edit-puzzle-tutorial') }}
          </b-button>
          </li>
        </ul>
      </SidebarPanel>
    </template>
  </EternaPage>
  <Preloader v-else style="margin-top: 10rem;" />
</template>

<script lang="ts">
  import { Component, Vue, Mixins } from 'vue-property-decorator';
  import { RouteCallback, Route } from 'vue-router';
  import axios from 'axios';
  import SidebarPanel from '@/components/Sidebar/SidebarPanel.vue';
  import EternaPage from '@/components/PageLayout/EternaPage.vue';
  import TagsPanel from '@/components/Sidebar/TagsPanel.vue';
  import Utils from '@/utils/utils';
  import { PUZZLE_ROUTE_PREFIX, PUZZLE_ROUTE_TUTORIAL_PREFIX } from '@/utils/constants';
  import Preloader from '@/components/PageLayout/Preloader.vue';
  import Comments from '@/components/PageLayout/Comments.vue';
  import FetchMixin from '@/mixins/FetchMixin';
  import { PuzzleResponse, Puzzle, CommentItem, ClearedPuzzle } from '@/types/common-types';

  const EDIT_PUZZLE_ROUTE = '/post/';

  @Component({
    components: {
      EternaPage,
      TagsPanel,
      SidebarPanel,
      Preloader,

    },
  })
  export default class PuzzleView extends Mixins(FetchMixin) {
    private puzzleRoute: string = PUZZLE_ROUTE_PREFIX;

    private tutorialRoute: string = PUZZLE_ROUTE_TUTORIAL_PREFIX;
    
    puzzle: Puzzle | null = null;

    nid: string = "";

    comments: CommentItem[] = [];

    clearedPuzzles: ClearedPuzzle[] = [];
    
    access: boolean = false;

    puzzTitle: string = "";

    puzzBody: string = "";

    async submit(){
      
      if(this.access && this.puzzTitle && this.puzzBody){
 
        this.$http.post('/post/', new URLSearchParams({
          type: 'edit_puzzle',
          nid: this.nid,
          title: this.puzzTitle,
          description: this.puzzBody,
        }))
          .then(res => {
            this.$router.push({path: `/puzzles/${this.nid}`});
          });
      }
    }

    async fetch() {
      const res = (
        await this.$http.get(`/get/?type=puzzle&nid=${this.$route.params.id}`)
      ).data.data as PuzzleResponse;

      if(this.$vxm.user.username !== res.puzzle.username){
        this.$router.push({path: `/puzzles/${res.nid}`});
      }
      this.access = true;
      this.puzzle = res.puzzle;
      this.nid = res.nid;
      this.comments = res.comments;
      this.clearedPuzzles = res.cleared || [];
      this.puzzTitle = res.puzzle.title;
      this.puzzBody = res.puzzle.body;

    }

    get madeByPlayer() {
      return this.puzzle && this.puzzle['made-by-player'] !== '0';
    }

    get clearedThisPuzzle() {
      return this.puzzle && this.clearedPuzzles.some(puzzle => this.nid === puzzle.id);
    }

    get imageURL() {
      return Utils.getPuzzleMiddleThumbnail(this.nid);
    }

    get avatar() {
      return Utils.getAvatar(this.puzzle?.userpicture || null);
    }
  }
</script>

<style scoped lang="scss">
  @import '@/styles/global.scss';


  .description-col {
    width: calc(60% - 15px);
  }

  .image-col {
    width: 40%;
  }

  @include media-breakpoint-down(xs) {
    .description-col, .image-col {
      width: 100%;
    }
  }

  .quest-image {
    margin: 15px;
  }

  .puzzle-image {
    width: 100%;
    background-color: #041227;
    border-radius: 5px;
    padding: 1.6rem 2.2rem;
    img {
      width: 100%;
      max-height: 400px;
    }
  }

  .puzzle-description {
    white-space: pre-wrap;
  }

  .submit-button {
    margin-top: 15px;
  }
  h2 {
    font-size: 22px;
    font-weight: bold;
  }
  h3 {
    font-size: 12px;
    text-transform: uppercase;
    font-weight: bold;
  }
  p {
    font-size: 17px;
    line-height: 1.47;
  }

  .icon {
    margin-right: 10px;
    width: 20.4px;
    height: 20px;
    object-fit: contain;
  }

  li {
    margin-bottom: 20px;
  }
</style>
