<template>
    <div class="home">
        <h1>부모</h1>
        <HomeChild v-bind:mySchoolProps="mySchool"></HomeChild>
    </div>
</template>
<script>
import HomeChild from './HomeChild.vue';

export default {
    name: "HomeParent",
    components: {
        HomeChild,
    },
    data() {
        return {
            myAge:40,
            mySchool:"싸피",
        }
    },
    created() {
        console.log(this.mySchool);
    }
}

</script>

<style>

</style>