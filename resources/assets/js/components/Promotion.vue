<template>
    <div class="promotion-box">
        <select :disabled="promotion.dates.startDate <= weeks[0].startDate ? true : false" v-if="edit == true" class="form-control select-dropdown" v-model="promotion.dates">
            <option v-for="week in weeks" :value="week">
                <h3>WEEK {{ moment(week.startDate, "w") }} - {{ moment(week.startDate, "Y") }}<h5> ({{ moment(week.startDate, "MMMM Do") }} - {{ moment(week.endDate, "MMMM Do") }})</h5></h3>
            </option>
        </select>
        <select v-else disabled class="form-control select-dropdown" v-model="promotion.dates">
            <option v-for="week in weeks" :value="week">
                <h3>WEEK {{ moment(week.startDate, "w") }} - {{ moment(week.startDate, "Y") }}<h5> ({{ moment(week.startDate, "MMMM Do") }} - {{ moment(week.endDate, "MMMM Do") }})</h5></h3>
            </option>
        </select>
        <div class="brand" v-if="edit == true">
            <div class="brand-image" :style="{'background-image': `url(${promotion.brand.image})`}"></div>
            <h4><h5>BRAND IMAGE URL</h5><input v-model="promotion.brand.image" type="text" name="" :value="promotion.brand.image"></h4><br>
            <h4><h5>BRAND NAME</h5><input v-model="promotion.brand.name" type="text" name="" :value="promotion.brand.name"></h4><br>
            <h5>BRAND DESCRIPTION</h5><p><textarea v-model="promotion.brand.description">{{ promotion.brand.description }}</textarea></p><br>
            <h4><h5>PROMOTION URL</h5><input v-model="promotion.link" type="text" name="" :value="promotion.link"></h4>
        </div>
        <div class="brand" v-else>
            <div class="brand-image" :style="{'background-image': `url(${promotion.brand.image})`}"></div>
            <h4>{{ promotion.brand.name }}</h4>
            <p>{{ promotion.brand.description }}</p>
        </div>
        <div class="signup-form">
            <button class="btn btn-primary" v-on:click="editPromotion" v-if="edit == true">
                DONE
            </button>
            <button class="btn btn-primary" v-on:click="editPromotion" v-else>
                EDIT
            </button>
            <button class="btn btn-primary" v-if="deletePromotionParent" v-on:click="deletePromotion(index)">
                DELETE
            </button>
        </div>
    </div>
</template>

<script>
    import moment from 'moment'
    // import autosize from 'autosize';
    export default {
        props: ['promotion', 'deletePromotionParent', 'sortPromotionsParent', 'index'],
        mounted() {
            console.log()
            this.getDates()
        },
        data() {
            return {
                edit: false,
                weeks: [],
            }
        },
        methods: {
            moment(date, format) {
                return moment(date).format(format)
            },
            getDates() {
                let weeks = []
                for (var i = -52; i < 52; i++) {
                    let week = {
                        startDate: moment().add(i, 'weeks').startOf('isoWeek').format('Y-MM-DD hh:mm:ss'),
                        endDate: moment().add(i, 'weeks').endOf('isoWeek').format('Y-MM-DD hh:mm:ss')
                    }
                    weeks.push(week)
                }
                this.weeks = weeks
            },
            editPromotion() {
                // autosize(document.querySelectorAll('textarea'));
                this.edit = !this.edit
                this.sortPromotionsParent()
            },
            deletePromotion(promotion) {
                let result = confirm("Delete this promotion?");
                if (result) {
                    this.deletePromotionParent(promotion)
                }   
            }
        }
    }
</script>
