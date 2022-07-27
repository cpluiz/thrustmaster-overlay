<template>
    <div class="group-container">
        <div class="btn btn-0" :class="{'selected':btn.btn0}"><Btn0></Btn0></div>
        <div class="btn btn-1" :class="{'selected':btn.btn1}"><Btn1></Btn1></div>
        <div class="btn btn-2" :class="{'selected':btn.btn2}"><Btn2></Btn2></div>
        <div class="btn btn-3" :class="{'selected':btn.btn3}"><Btn3></Btn3></div>
        <div class="btn btn-4" :class="{'selected':btn.btn4}"><Btn4></Btn4></div>
        <div class="btn btn-5" :class="{'selected':btn.btn5}"><Btn5></Btn5></div>
    </div>
</template>
<script>
import Btn0 from "./../assets/btn0.vue";
import Btn1 from "./../assets/btn1.vue";
import Btn2 from "./../assets/btn2.vue";
import Btn3 from "./../assets/btn3.vue";
import Btn4 from "./../assets/btn4.vue";
import Btn5 from "./../assets/btn5.vue";
export default{
    props: ["buttons"],
    data() {
        return {
            data: null
        };
    },
    computed: {
        btn(){
            let buttons = {
                btn0: false,
                btn1: false,
                btn2: false,
                btn3: false,
                btn4: false,
                btn5: false,
            };
            if(this.buttons.btn0 !== undefined){
                buttons.btn0 = this.buttons.btn0.pressed;
                buttons.btn1 = this.buttons.btn1.pressed;
                buttons.btn2 = this.buttons.btn2.pressed;
                buttons.btn3 = this.buttons.btn3.pressed;
                buttons.btn4 = this.buttons.btn4.pressed;
                buttons.btn5 = this.buttons.btn5.pressed;
            }
            return buttons;
        }
    },
    components: { Btn0, Btn1, Btn2, Btn3, Btn4, Btn5 }
}
</script>
<style lang="scss">
.group-container{
    position: relative;
    width: 200px;
    .btn{
        position:absolute;
        &.selected{
            svg{
                path{
                    fill: orangered !important
                };
                g{
                    fill: #353535 !important;
                }
            }
        }
        &.btn-0{top: 0; left: 0;}
        &.btn-1{top: 0; left: 38px;}
        &.btn-2{top: 1px; left: 82px;}
        &.btn-3{top: 51px; left: 99px;}
        &.btn-4{top: 52px; left: 55px;}
        &.btn-5{top: 52px; left: 8px;}
    }
}
</style>