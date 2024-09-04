<template>
    <div>
        손자
        {{ $store.state.myAge }} <br />
        단축이름으로 store.state를 사용하지 않고 local 변수처럼 사용 : {{ myAge }}
        {{ myName }}
    </div>

</template>

<script>
import { mapState } from 'vuex';

// import {mapState} from 'vuex'
export default {
    props: ['mySchoolProps'],
    computed:{
        ...mapState(['myAge','myName'])
    }, // mapState 를 사용하여 store의 값을 단축이름으로 사용가능하다 
    created() {

    }
}

</script>

<style>

</style>