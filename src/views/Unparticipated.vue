<template>
    <div>
        <vue-overwatch-loading :Radius="45" Color="#ffff66"
                               class="loading"
                               v-if="$store.state.activity.activities.length === 0"></vue-overwatch-loading>
        <UnparticipatedActivity
                v-if="$store.getters.logged"
                :item="item"
                :key="item.id"
                v-for="item in $store.getters.getActivitiesForType($route.params.id)">
        </UnparticipatedActivity>
        <Activity :item="item" :key="item.id"
                  v-else
                  v-for="item in $store.getters.getActivitiesForType($route.params.id)">
            <v-card-actions class="justify-center">
                <v-btn class="third" color="fourth" to="/login">请先登录</v-btn>
            </v-card-actions>
        </Activity>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from "vue-property-decorator";
    import UnparticipatedActivity from "@/components/activity/shuZhiNet/unparticipatedActivity.vue";
    import Activity from "@/components/activity/shuZhiNet/activity.vue";
    import {frontendTypeIdToBackendTypeId} from "@/store/module/activity";

    @Component({
        components: {
            UnparticipatedActivity,
            Activity
        }
    })
    export default class Unparticipated extends Vue {
        private frontendTypeIdToBackendTypeId = frontendTypeIdToBackendTypeId;

        private async mounted() {
            if (this.$store.state.activity.activities.length === this.$store.getters.getParticipated.length) {
                await this.$store.dispatch("fetchActivities");
            }
        }
    }
</script>

<style lang="stylus" scoped>
    .loading {
        display flex
        justify-content center
    }
</style>
