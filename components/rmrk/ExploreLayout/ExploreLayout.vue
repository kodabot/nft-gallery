<template>
  <div>
    <div v-if="$route.query.search" class="block">
      Showing results for
      <span class="text__stroked is-size-3">{{ $route.query.search }}</span>
    </div>
    <b-tabs v-model="selectedTab">
      <b-tab-item label="Collections" value="COLLECTION">
        <CollectionList v-if="type === 'rmrk'" />
        <Collections v-if="type !== 'rmrk'" />
      </b-tab-item>
      <b-tab-item label="Gallery" value="GALLERY"><Gallery /></b-tab-item>
    </b-tabs>
  </div>
</template>

<script lang="ts">
import { Component, mixins } from 'nuxt-property-decorator'
import CollectionList from '@/components/rmrk/Collection/List/CollectionList.vue'
import Collections from '@/components/rmrk/Gallery/Collections.vue'
import PrefixMixin from '~/utils/mixins/prefixMixin'

const components = {
  CollectionList,
  Collections,
}

@Component<ExploreLayout>({
  components,
})
export default class ExploreLayout extends mixins(PrefixMixin) {
  get type() {
    return this.urlPrefix
  }
  get selectedTab(): string {
    return (this.$route.query.tab as string) || 'COLLECTION'
  }

  set selectedTab(val) {
    this.$route.query.page = ''
    let queryOptions: { tab: string; search?: string | (string | null)[] } = {
      tab: val,
    }
    if (this.$route.query.search) {
      queryOptions.search = this.$route.query.search
    }

    this.$router.replace({
      query: queryOptions,
    })
  }
}
</script>
