<template>
    <b-container>
        <b-row>

            <b-col cols="8" class="mt-4">
                <h3>Khóa : {{ courseName }}
                </h3>
                <b-table :fields="fields" :items="listVideo" show-empty>
                    <template #cell(index)="data">
                        {{ data.index + 1 }}
                    </template>
                    <template #cell(name)="data">
                        {{ data.item.name }}
                    </template>
                    <template #cell(avatar)="data">
                        <b-img-lazy width="200px" height="200px" :src="data.item.avatar"></b-img-lazy>
                    </template>
                    <template #cell(tool)="data">
                        <b-button :href="data.item.link" variant="primary" block target="_blank">Go</b-button>
                        <br />

                        <b-button block @click="deleteVideo(data.item.key)" variant="danger">Delete</b-button>

                    </template>

                </b-table>
            </b-col>

            <b-col cols="4" class="mt-4">
                <h3>
                    Thêm Video
                </h3>
                <b-overlay :show="overlay">
                    <b-form @submit.stop.prevent="addNew">
                        <label>Tên Video:</label>
                        <b-form-input required autocomplete="off" v-model="video.name"></b-form-input>

                        <label>Link thumbnail:</label>
                        <b-form-input required autocomplete="off" v-model="video.avatar"></b-form-input>
                        <b-img-lazy class="mt-2" width="200px" height="200px" :src="video.avatar">

                        </b-img-lazy>
                        <br />
                        <label class="mt-2">Link Video:</label>
                        <b-form-input required autocomplete="off" v-model="video.link"></b-form-input>
                        <b-button type="submit" variant="success">Thêm Video</b-button>
                        <b-embed class="mt-4" type="iframe" aspect="16by9" :src="video.link" allowfullscreen></b-embed>

                    </b-form>

                </b-overlay>
            </b-col>

        </b-row>
    </b-container>
</template>

<script>
export default {
    data() {
        return {
            courseName: '',
            overlay: false,
            listVideo: [],
            video: {
                name: null,
                avatar: null,
                link: null
            },
            fields: [
                { key: 'index', label: "#" },
                { key: 'name', label: "Name" },
                { key: 'avatar', label: "Thumb" },
                { key: 'tool', label: "Tool" },
            ]
        }
    },
    methods: {
        deleteVideo(key) {
            let url = 'https://database.deta.sh/v1/c07frcmppve/courseVideo/items/' + key
            let body = {

            }
            fetch(url, {
                method: 'DELETE',
                headers: {
                    "Accept": "application/json",
                    "Content-Type": "application/json",
                    "X-API-Key": "c07frcmppve_xV1VFAkqtTZtaX8eo1TJ6VjT1XuDDHGb",
                },
                body: JSON.stringify(body),
            }).then(data => data.json()).then(data => {

                this.getListVideo(this.courseName)

            })
        },
        addNew() {
            this.overlay = true;
            let url = 'https://database.deta.sh/v1/c07frcmppve/courseVideo/items'
            let body = {
                items: [
                    {
                        course: this.courseName,
                        avatar: this.video.avatar,
                        link: this.video.link,
                        name: this.video.name
                    }]
            }
            fetch(url, {
                method: 'PUT',
                headers: {
                    "Accept": "application/json",
                    "Content-Type": "application/json",
                    "X-API-Key": "c07frcmppve_xV1VFAkqtTZtaX8eo1TJ6VjT1XuDDHGb",
                },
                body: JSON.stringify(body),
            }).then(data => data.json()).then(data => {
                this.overlay = false;
                this.getListVideo(this.courseName)
            })
        },
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

<style>
.plusAdd {
    color: green;
    font-size: large;
    cursor: pointer;
}

.plusAdd:hover {
    color: red;
}
</style>