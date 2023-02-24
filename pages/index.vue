<template>
  <b-container class="mt-4">
    <b-row>
      <b-col cols="6" v-for="(item, index) in khoahoc" :key="index">
        <div>
          <b-card :img-src="item.avatar" :title="item.name" :sub-title="item.author">
            <b-card-text>
              {{ item.desc }}
            </b-card-text>
            <b-button :href="'video?id=' + item.key" variant="primary">Tập bài này</b-button>
          </b-card>
        </div>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  components: {

  },
  data() {
    return {
      key: 'c07frcmppve_xV1VFAkqtTZtaX8eo1TJ6VjT1XuDDHGb',
      baseURL: 'https://database.deta.sh/v1/{project_id}/{base_name}',

      khoahoc: []
    }
  },
  mounted() {
    this.getData();
  },
  methods: {
    getData() {

      let url = 'https://database.deta.sh/v1/c07frcmppve/course/query'
      let body = {

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

        listData.map(item => {
          this.khoahoc.push({ ...item })
        })
      })
    }
  }
}
</script>

<style></style>
