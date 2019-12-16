<template>
	<view>
        <picker mode="multiSelector" 
                :value="multiIndex" 
                :range="multiArray" 
                @change="handleValueChange"
                @columnchange="handleColumnChange">
            <slot></slot>
        </picker>
	</view>
</template>

<script>
    import china_regions from './china_regions.js'
    const provinceArr = Object.keys(china_regions)
	export default {
		data() {
			return {
                cityArr:[],
                districtArr:[],
                multiIndex: [0, 0, 0],
                isInitMultiArray:false
			}
		},
        computed:{
            // 依赖式改变
            multiArray(){
                // 进行初始化
                if(this.isInitMultiArray){
                    let firstCityArr = Object.keys(china_regions[provinceArr[0]]);
                    return [
                        provinceArr,
                        firstCityArr,
                        china_regions[provinceArr[0]][firstCityArr[0]]
                    ]
                }
                return [provinceArr,this.cityArr,this.districtArr];
            },
        },
		methods: {
			handleValueChange(e){
                // 结构赋值
                let [index0,index1,index2] = e.detail.value;
                let [arr0,arr1,arr2] = this.multiArray;
                let address = [arr0[index0],arr1[index1],arr2[index2]];
                // console.log(address);
                this.$emit('getAddress',address)
            },
            handleColumnChange(e){
                // console.log(e);
                this.isInitMultiArray = false;
                const that = this;
                let col = e.detail.column;
                let row = e.detail.value;
                that.multiIndex[col] = row;
                let province = provinceArr[that.multiIndex[0]];
                let city = Object.keys(china_regions[province])[that.multiIndex[1]];
                switch(col){
                    case 0:
                        // 第一列change,二三列联动
                        that.cityArr =Object.keys(china_regions[province]);
                        that.districtArr = china_regions[province][that.cityArr[that.multiIndex[2]]];
                        break;
                    case 1:
                        // 第二列change，第三列联动
                        that.districtArr =  china_regions[province][city];
                        break;
                    case 2:
                        break;
                }
            },
		},
        mounted(){
            // console.log('init');
            this.isInitMultiArray = true;
        }
	}
</script>

<style>

</style>
