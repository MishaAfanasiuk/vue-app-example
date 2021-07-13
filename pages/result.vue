<template>
  <div>
    <Menu/>
    <List :data='data' :fields='fields' :current-page="currentPage" :items-per-page="itemsPerPage" :set-current-page="setCurrentPage" />
  </div>
</template>

<script lang='ts'>
import { Component, Vue } from 'vue-property-decorator'
import captureException from '~/utils/captureException'

interface DataItem {
  userId: string;
  id: string;
  title: string;
  body: string;
}

@Component({
  async fetch() {
    try {
      this.data = await this.$axios.$get(`https://jsonplaceholder.typicode.com/posts/`)
    } catch (e) {
      captureException(e)
    }
  }
})
export default class Form extends Vue {
  private data: DataItem[] | null = null;
  private fields = ['userId', 'id', 'title', 'body']
  private currentPage = 1;
  private itemsPerPage = 10;

  private setCurrentPage(page: number) {
    this.currentPage = page;
  }
}
</script>
