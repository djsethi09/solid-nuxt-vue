<!--
1. This componet does it all - Fetches the photos, displays them in list, does a complete crud here -SRP ->divide list, divide dialog, crud methods in separate files
2. This component is inflexible to show Cards instead of List of photos -> OCP -> use Slot in Home.vue, PhotosList -> <slot/> -> PhotoTable, PhotoGrid
3. The API methods will be fetch and axios -> but we have to have a single parent class- should be independent of axios vs fetch - LSP
4. ISP - both Card and Row component should not have all the fields passed to components, just pass required props
5. DIP - All the API classes should implement API interface, and Home.vue should depend on IAPI, rather than API. Depend on Abstractions 
-->
<template>
  <div>
    <Photos>
      <PhotosList :photos="photos" :headers="headers" />
    </Photos>
    <br />
    <br />
    <hr />
    <Photos>
      <PhotosGrid :photos="photos" :headers="headers" />
    </Photos>
  </div>
</template>
<script lang="ts">
import { Vue, Component, Prop } from "vue-property-decorator";
import Photos from "@/components/Photos.vue";
import PhotosList from "@/components/PhotosList.vue";
import PhotosGrid from "@/components/PhotosGrid.vue";
import { Api } from "@/api/Api";
import { IPhoto } from "../types/Photo";
import IApi from "../types/IApi";
@Component({
  components: {
    Photos,
    PhotosList,
    PhotosGrid
  }
})
export default class Index extends Vue implements IApi {
  photos: any[] = [];
  headers: any[] = [
    {
      text: "Id",
      align: "start",
      sortable: false,
      value: "id"
    },
    { text: "Title", value: "title", sortable: true },
    { text: "Thumbnail", value: "thumbnailUrl" }
  ];
  created() {
    //this.initialize();
  }
  async mounted() {
    this.photos = await this.fetchPhotos();
  }
  async fetchPhotos(): Promise<IPhoto[]> {
    return await this.fetch("albums/1/photos");
  }
  async fetch(url: string): Promise<IPhoto[]> {
    const api = new Api();
    return await api.fetch(url);
  }
}
</script>