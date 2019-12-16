<template>
    <view>
        <view><h1>TEST</h1></view>
        <regions @getAddress="handleAdress">
            <view><h2>选择</h2></view>
        </regions>
    </view>
</template>

<script>
    import regions from './regions'
    export default {
        components:{
            regions
        },
        data(){
            return {
                
            }
        },
        methods:{
            handleAdress(val){
                console.log(val);
            }
        }
    }
</script>

<style>
</style>
