<template>
    <div>
        <form @submit.prevent="createEvent">
            <BaseSelect label="Select a category" :options="categories" v-model="event.category" />
            <h3>Name & describe your event</h3>
            <BaseInput type="text" label="Title" v-model="event.title" placeholder="Event Title" />
            <BaseInput
                type="text"
                label="Description"
                v-model="event.description"
                placeholder="Add a description"
            />
            <h3>Where is your event?</h3>
            <BaseInput
                type="text"
                label="Location"
                v-model="event.location"
                placeholder="Add a location"
            />
            <h3>When is your event?</h3>
            <div class="field">
                <label>Date</label>
                <datepicker v-model="event.date" placeholder="Select a date" />
            </div>
            <BaseSelect label="Select a time" :options="times" v-model="event.time" />
            <!-- <input type="submit" class="button -fill-gradient" value="Submit" /> -->
            <BaseButton type="submit" buttonClass="-fill-gradient">Submit</BaseButton>
        </form>
    </div>
</template>

<script>
import { mapState, mapGetters } from 'vuex';
import Datepicker from 'vuejs-datepicker';
import NProgress from 'nprogress';

export default {
    computed: {
        ...mapState(['categories'])
    },
    components: {
        Datepicker
    },
    data() {
        const times = [];
        for (let i = 1; i <= 24; i++) {
            times.push(i + ':00');
        }
        return {
            times,
            event: this.createFreshEventObject()
        };
    },
    methods: {
        createFreshEventObject() {
            const user = this.$store.state.user.user;
            const id = Math.floor(Math.random() * 10000000);

            return {
                id: id,
                user: user,
                category: '',
                organizer: user,
                title: '',
                description: '',
                location: '',
                date: '',
                time: '',
                attendees: []
            };
        },
        createEvent() {
            NProgress.start();

            this.$store
                .dispatch('createEvent', this.event)
                .then(() => {
                    this.$router.push({
                        name: 'event-show',
                        params: { id: this.event.id }
                    });
                    this.event = this.createFreshEventObject();
                })
                .catch(() => {
                    NProgress.done();
                });
        }
    }
};
</script>

<style scoped>
.field {
    margin-bottom: 24px;
}
</style>