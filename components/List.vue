<template>
  <div v-if="data">
    <b-table
      :sticky-header="false"
      :no-border-collapse="false"
      :items="data.slice(itemsPerPage * (currentPage - 1), itemsPerPage * currentPage)"
      :fields="fields"
    >
      <template #head()="scope">
        <div class="text-nowrap">
          {{ scope.label }}
        </div>
      </template>
    </b-table>

    <b-pagination
      :value="currentPage"
      :total-rows="data.length"
      :per-page="itemsPerPage"
      aria-controls="my-table"
      @change="setCurrentPage"
    ></b-pagination>
  </div>
</template>

<script lang='ts'>
import { Component, Vue, Prop } from 'vue-property-decorator'

@Component({
  name: 'List'
})
export default class List extends Vue {
  @Prop({ required: true }) readonly data!: string
  @Prop({ required: true }) readonly fields!: string[];
  @Prop({ required: true }) readonly currentPage!: number;
  @Prop({ required: true }) readonly itemsPerPage!: number;
  @Prop({ required: true }) readonly setCurrentPage!: (page: number) => void;
}
</script>
