<template>
    <div>
        <div>
            <Checkbox
                :indeterminate="indeterminate"
                :value="checkAll"
                @click.prevent.native="handleCheckAll">全选</Checkbox>
            <CheckboxGroup v-model="checkAllGroup" @on-change="checkAllGroupChange">
                <Checkbox label="东门" v-model="checkEast"></Checkbox>
                <Checkbox label="西门" v-model="checkWest"></Checkbox>
                <Checkbox label="南门" v-model="checkSouth"></Checkbox>
                <Checkbox label="北门" v-model="checkNorth"></Checkbox>
            </CheckboxGroup>
        </div>
        

        <div style="margin-top:20px;">
            <Form ref="formCustom" :model="formCustom" :rules="ruleCustom" :label-width="100">
                <FormItem label="东门卡号：" prop="number1">
                    <Col span="6"><Input v-model="formCustom.number1" :disabled="!checkEast" placeholder="请输入东门卡号" type="text"/></Col>
                </FormItem> 
                <FormItem label="西门卡号：" prop="number2">
                    <Col span="6"><Input v-model="formCustom.number2" :disabled="!checkWest" placeholder="请输入西门卡号" type="text"/></Col>
                </FormItem> 
                <FormItem label="南门卡号：" prop="number3">
                    <Col span="6"><Input v-model="formCustom.number3" :disabled="!checkSouth" placeholder="请输入南门卡号" type="text"/></Col>
                </FormItem>
                <FormItem label="北门卡号：" prop="number4">
                    <Col span="6"><Input v-model="formCustom.number4" :disabled="!checkNorth" placeholder="请输入北门卡号" type="text"/></Col>
                </FormItem>
            </Form>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            indeterminate: true,
            checkAll: false,
            checkEast:false,
            checkWest:false,
            checkSouth:false,
            checkNorth:false,
            checkAllGroup: [],
            disabledf: true,
            formCustom: {
                number1:'',
                number2:'',
                number3:"",
                number4:"",

            },
            ruleCustom: {

            }


        }
    },
    methods: {
        handleCheckAll () {
            if (this.indeterminate) {
                this.checkAll = false;
            } else {
                this.checkAll = !this.checkAll;
            }
            this.indeterminate = false;

            if (this.checkAll) {
                this.checkAllGroup = ['东门', '西门', '南门','北门'];
                this.checkEast=true;
                this.checkWest=true;
                this.checkSouth=true;
                this.checkNorth=true;
            } else {
                this.checkAllGroup = [];
            }
        },
        checkAllGroupChange (data) {
            if (data.length === 4) {
                this.indeterminate = false;
                this.checkAll = true;
            } else if (data.length > 0) {
                this.indetesminate = true;
                this.checkAbl = false;
            } else {
                this.indetersminate = false;
                this.checkAbl = false;
            }
        }
    }
}
</script>

<style>

</style>
