<template>
    <b-container>
        <b-row>
            <b-col cols="3">
                <div class="mt-5">
                    <h3>Ảnh đại diện</h3>
                    <img :src="courseInfo.avatar" width="200px" height="200px">
                </div>
                <div class="mt-5">
                    <h3>Lịch tập</h3>
                    <img :src="courseInfo.schedule" width="200px" height="200px">
                </div>

            </b-col>
            <b-col cols="6" class="mt-4">
                <b-overlay :show="overlay">
                    <b-form @submit.stop.prevent="addNew">
                        <label>Tên khóa học:</label>
                        <b-form-input required autocomplete="off" v-model="courseInfo.name"></b-form-input>
                        <label>Ảnh đại diện:</label>
                        <b-form-input required autocomplete="off" v-model="courseInfo.avatar"></b-form-input>
                        <label>Tác giả:</label>
                        <b-form-input required autocomplete="off" v-model="courseInfo.author"></b-form-input>
                        <label>Mô tả:</label>
                        <b-textarea required autocomplete="off" v-model="courseInfo.desc"></b-textarea>
                        <label>Lịch Tập:</label>
                        <b-form-input required autocomplete="off" v-model="courseInfo.schedule"></b-form-input>
                        <b-button class="mt-4" variant="success" type="submit" block>Thêm khóa</b-button>
                    </b-form>
                    <br />
                </b-overlay>

            </b-col>
        </b-row>
    </b-container>
</template>

<script>
export default {
    data() {
        return {
            overlay: false,
            courseInfo: {
                name: null,
                avatar: null,
                author: null,
                desc: null,
                schedule: null
            }
        }
    },
    methods: {
        addNew() {
            this.overlay = true;
            let url = 'https://database.deta.sh/v1/c07frcmppve/course/items'
            let body = {
                items: [
                    {

                        name: this.courseInfo.name,
                        desc: this.courseInfo.desc,
                        avatar: this.courseInfo.avatar,
                        schedule: this.courseInfo.schedule,
                        author: this.courseInfo.author,
                        key: (this.courseInfo.name).replace(/\s+/g, '').toLowerCase()
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
            })
        },

    },
    mounted() {

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