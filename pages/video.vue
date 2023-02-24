<template>
  <b-container class="mt-4">

    <b-row>
      <b-col cols="6">
        <h3>Khóa tập : {{ courseName }}</h3>


      </b-col>
      <b-col cols="6">
        <b-button variant="primary" :href="'shedule?id=' + courseName">Xem lịch tập</b-button>

        <b-button :href="'addVideo?id=' + courseName" variant="warning">Thêm Video</b-button>
      </b-col>
      <b-col class="mt-4" v-for="(item, index) in listVideo" :key="index" cols="4">
        <b-card :title="item.name" :img-src="item.avatar" img-top tag="article" style="max-width: 20rem;" class="mb-2">
          <b-button :href="item.link" variant="primary">Go</b-button>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      listVideo: [],
      courseName: null,

    }
  },
  methods: {
    getListVideo(name) {

      let url = 'https://database.deta.sh/v1/c07frcmppve/courseVideo/query'
      let body = {
        "query": [
          {
            course: name
          }
        ]
      }
      fetch(url, {
        method: 'POST',
        headers: {
          "Accept": "application/json",
          "Content-Type": "application/json",
          "X-API-Key": "c07frcmppve_xV1VFAkqtTZtaX8eo1TJ6VjT1XuDDHGb",
        },
        body: JSON.stringify(body),
      }).then(data => data.json()).then(data => {
        let listData = data.items;
        this.listVideo = listData

      })
    }
  },
  mounted() {
    this.courseName = this.$route.query.id
    this.getListVideo(this.courseName)
  }

}
</script>

<style></style>