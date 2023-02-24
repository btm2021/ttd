
<template>
    <div v-if="item">

        <b-img-lazy style="width:90vw;heigh:90vh" :src="item.schedule">

        </b-img-lazy>
    </div>
</template>

<script>
export default {
    data() {
        return {
            item: null,
            courseName: ''
        }
    },
    methods: {
        getData(name) {
            let url = 'https://database.deta.sh/v1/c07frcmppve/course/query'
            let body = {
                "query": [
                    {
                        key: name
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
                this.item = listData[0]
                console.log(this.item)
            })
        }
    },
    mounted() {
        this.courseName = this.$route.query.id
        this.getData(this.courseName)
    }
}
</script>

<style></style>