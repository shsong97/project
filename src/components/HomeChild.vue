<template>
    <div>
        <h1>자식</h1>
        <HomeGrandChild />
    </div>
</template>
<script>
import HomeGrandChild from './HomeGrandChild.vue';

export default {
    props:['mySchoolProps'],
    components: {
        HomeGrandChild,
    },
    created() {
        console.log('---자식---')
        console.log(this.mySchool);
        console.log(this.mySchoolProps);
        console.log('---자식---');
    }
}
</script>

<style>

</style>