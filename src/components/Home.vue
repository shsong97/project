<template>
    <div class="home">
        
        <p>HomeChild</p>
        <HomeChild v-bind:mySchoolProps="mySchool"></HomeChild>

        <p>GeoView</p>
        <GeoView></GeoView>
        
        <p>TestPage</p>
        <TestPage></TestPage>

        <p>TableList View</p>
        <TableList></TableList>
    </div>
</template>
<script>
import GeoView from './GeoView.vue';
import HomeChild from './HomeChild.vue';
import TableList from './TableList.vue';
import TestPage from './TestPage.vue';

export default {
    name: "HomeParent",
    components: {
        HomeChild,
        GeoView,
        TableList,
        TestPage,
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