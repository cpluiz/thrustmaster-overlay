<template>
    <div class="button-holder">
        <div class="button button-top" :class="{'selected':axeToPOV.up.pressed}">
            <CirclePart></CirclePart>
        </div>
        <div class="button button-right" :class="{'selected':axeToPOV.right.pressed}">
            <CirclePart></CirclePart>
        </div>
        <div class="button button-down" :class="{'selected':axeToPOV.down.pressed}">
            <CirclePart></CirclePart>
        </div>
        <div class="button button-left" :class="{'selected':axeToPOV.left.pressed}">
            <CirclePart></CirclePart>
        </div>
    </div>
</template>
<script>
import CirclePart from "../assets/CirclePart.vue";
export default{
    props: ["buttons", "pov1"],
    computed:{
        axeToPOV(){
            let buttons = {
                up:{pressed: false},
                right:{pressed: false},
                down:{pressed: false},
                left:{pressed: false}
            }
            if(this.buttons !== undefined && this.buttons.up !== undefined){
                buttons.up.pressed = this.buttons.up.pressed;
                buttons.down.pressed = this.buttons.down.pressed;
                buttons.left.pressed = this.buttons.left.pressed;
                buttons.right.pressed = this.buttons.right.pressed;
            }
            if(this.pov1 !== undefined){
                buttons.up.pressed = (this.pov1 <= -0.75 || this.pov1 >= 0.75) && !(this.pov1 > 1 || this.pov1 < -1);
                buttons.right.pressed = this.pov1 >= -0.75 && this.pov1 <= -0.1;
                buttons.down.pressed = this.pov1 >= -0.15 && this.pov1 <= 0.4;
                buttons.left.pressed = this.pov1 >= 0.4 && this.pov1 <= 0.75;
            }
            return buttons;
        }
    },
    components: { CirclePart }
}
</script>
<style lang="scss">
.button-holder{
    position: relative;
    display: block;
    width: 42px;
    height: 42px;
    .button{
        position: absolute;
        width: 21px;
        height: 21px;
        svg{
            max-width: 100%;
            max-height: 100%;
        }
        &.button-top{
            top: 1px;
            left: 50%;
            transform: translateX(-50%);
            svg{
                transform: rotate(-90deg)
            }
        }
        &.button-right{
            right: 1px;
            top: 50%;
            transform: translateY(-50%);
        }
        &.button-down{
            bottom: 1px;
            left: 50%;
            transform: translateX(-50%);
            svg{
                transform: rotate(90deg)
            }
        }
        &.button-left{
            left: 1px;
            top: 50%;
            transform: translateY(-50%);
            svg{
                transform: rotate(180deg)
            }
        }
        &.selected{
            svg{
                path{fill: orangered !important;}
            }
        }
    }
}
</style>