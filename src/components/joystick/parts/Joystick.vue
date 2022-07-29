<template>
    <div class="joystick-base" v-if="visible">
        <div class="group1">
            <ButtonGroup v-bind:buttons="classGroup1"></ButtonGroup>
        </div>
        <div class="group2">
            <ButtonGroup v-bind:buttons="classGroup2"></ButtonGroup>
        </div>
        <div class="analog-block">
            <div class="container">
                <div class="joystick-analog">
                    <AnalogPad v-bind:x="axes.x" v-bind:y="axes.y">
                        <div class="head-area" :style="{'--rotation' : rotation}">
                            <div class="container">
                                <div class="joystick-trigger">
                                    <div class="push-button trigger" :class="{'selected': btns.button1 !== undefined && btns.button1.pressed}">
                                        <div class="button-container">
                                            <ThrusterButton></ThrusterButton>
                                        </div>
                                    </div>
                                </div>
                                <div class="joystick-head">
                                    <JoystickHead></JoystickHead>
                                </div>
                                <div class="joystick-pov1">
                                    <CircleGroup v-bind:pov1="axes.pov1"></CircleGroup>
                                </div>
                                <div class="joystick-thumb" :class="{'selected': btns.button2 !== undefined && btns.button2.pressed}">
                                    <JoystickThumb></JoystickThumb>
                                </div>
                                <div class="joystick-left" :class="{'selected': btns.button3 !== undefined && btns.button3.pressed}">
                                    <JoystickSide></JoystickSide>
                                </div>
                                <div class="joystick-right" :class="{'selected': btns.button4 !== undefined && btns.button4.pressed}">
                                    <JoystickSide></JoystickSide>
                                </div>
                            </div>
                        </div>
                    </AnalogPad>
                </div>
                <div class="joystick-scroll">
                    <div class="container">
                        <div class="slider-bg">
                            <SliderBG></SliderBG>
                        </div>
                        <AnalogPad v-bind:y="axes.scroll">
                            <div class="container">
                                <div class="knob"><SliderKnob></SliderKnob></div>
                            </div>
                        </AnalogPad>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script>    
import ButtonGroup from './subparts/ButtonGroup.vue';
import AnalogPad from './subparts/AnalogPad.vue';
import JoystickHead from './assets/JoystickHead.vue';
import JoystickThumb from './assets/JoystickThumb.vue';
import JoystickSide from './assets/JoystickSide.vue';
import ThrusterB2 from './assets/ThrusterB2.vue';
import ThrusterB1 from './assets/ThrusterB1.vue';
import ThrusterButton from './assets/ThrusterButton.vue';
import CircleGroup from './subparts/CircleGroup.vue';
import SliderBG from './assets/SliderBG.vue';
import SliderKnob from './assets/SliderKnob.vue';
export default{
    props: ["buttons", "axes"],
    data() {
        return {
            timestamp: 0
        };
    },
    computed: {
        classGroup1() {
            let buttons = {
                btn0: undefined,
                btn1: undefined,
                btn2: undefined,
                btn3: undefined,
                btn4: undefined,
                btn5: undefined,
            };
            if (this.buttons !== undefined) {
                buttons.btn0 = this.buttons.button5;
                buttons.btn1 = this.buttons.button6;
                buttons.btn2 = this.buttons.button7;
                buttons.btn3 = this.buttons.button8;
                buttons.btn4 = this.buttons.button9;
                buttons.btn5 = this.buttons.button10;
            }
            return buttons;
        },
        classGroup2() {
            let buttons = {
                btn0: undefined,
                btn1: undefined,
                btn2: undefined,
                btn3: undefined,
                btn4: undefined,
                btn5: undefined,
            };
            if (this.buttons !== undefined) {
                buttons.btn0 = this.buttons.button11;
                buttons.btn1 = this.buttons.button12;
                buttons.btn2 = this.buttons.button13;
                buttons.btn3 = this.buttons.button14;
                buttons.btn4 = this.buttons.button15;
                buttons.btn5 = this.buttons.button16;
            }
            return buttons;
        },
        btns(){
            return this.buttons;
        },
        rotation(){
            if(this.axes.twist === undefined) return '0deg';
            return (this.axes.twist * 35)+"deg";
        },
        show(){
            return this.axes.x < -0.05 || this.axes.x > 0.05 ||
                    this.axes.y < -0.05 || this.axes.y > 0.05 ||
                    this.axes.twist < -0.05 || this.axes.twist > 0.05 ||
                    (this.axes.pov1 < 1 && this.axes.pov1 > - 1) ||
                    this.buttons.button1.pressed ||
                    this.buttons.button2.pressed ||
                    this.buttons.button3.pressed ||
                    this.buttons.button4.pressed ||
                    this.buttons.button5.pressed ||
                    this.buttons.button6.pressed ||
                    this.buttons.button7.pressed ||
                    this.buttons.button8.pressed ||
                    this.buttons.button9.pressed ||
                    this.buttons.button10.pressed ||
                    this.buttons.button11.pressed ||
                    this.buttons.button12.pressed ||
                    this.buttons.button13.pressed ||
                    this.buttons.button14.pressed ||
                    this.buttons.button15.pressed ||
                    this.buttons.button16.pressed;
        },
        visible(){
            return this.show || this.timestamp > 0;
        },
    },
    methods:{
        tick(){
            if(this.show){
                this.timestamp = 60 * 3;
            }
            if(this.timestamp > -1)
                this.timestamp --;
        },
    },
    components: { ButtonGroup, AnalogPad, JoystickHead, JoystickThumb, JoystickSide, ThrusterB2, ThrusterB1, ThrusterButton, CircleGroup, SliderBG, SliderKnob }
}
</script>

<style lang="scss">
.joystick-base{
    position: relative;
    width: 500px;
    .group1{
        top: 2rem;
    }
    .group2{
        position: absolute;
        top: 2rem;
        left: 18.9rem;
        transform: scaleX(-100%);
    }
    .analog-block{
        position: absolute;
        top: 0;
        left: 50%;
        transform: translateX(-50%);
        width: 100px;
        .container{
            position: relative;
            width: 46px;
            height: 128px;
            .joystick-analog{
                position: absolute;
                top: 3rem;
                left: 0;
                width: 100px;
                height: 100px;
            }
            .joystick-scroll{
                position: absolute;
                top: 1.5rem;
                right: -18rem;
                width: calc(1rem + 4px);
                height: 100px;
                .container{
                    position: relative;
                    padding-top: 10px;
                    .analog-container{
                        height: 110px;
                    }
                    .slider-bg{
                        position: absolute;
                        height: 100%;
                    }
                    .analog-pad{
                        width: 29px;
                        height: 20px;
                        transform: translate(-50%, 0);
                        &:before{
                            display: none;
                        }
                        .container{
                            position: relative;
                            width: 100%;
                            height: 100%;
                            transform: translateY(-50%);
                        }
                    }
                }
            }
        }
    }
    .head-area{
        position: absolute;
        width: 68px;
        height: 61px;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%) scale(1.5) rotate(var(--rotation));
        .container{
            position: relative;
            width: 100%;
            height: 100%;
        }
        .joystick-head{
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translate(-50%);
            width: 100%;
        }
        .joystick-thumb{
            position: absolute;
            bottom: 1px;
            left: 50%;
            transform: translate(-50%);
        }
        .joystick-left{
            position: absolute;
            top: 6px;
            left: 0;
        }
        .joystick-right{
            position: absolute;
            top: 6px;
            right: 0px;
            transform: scaleX(-100%);
        }
        .joystick-trigger{
            position: absolute;
            top: -4px;
            left: 50%;
            transform: translateX(-50%);
            .push-button{
                &.selected{
                    .button-container{
                        padding-top: 3px;
                    }
                }
            }
        }
        .joystick-pov1{
            position: absolute;
            top: 0;
            left: 50%;
            transform: translate(-50%, -10%) scale(0.45, 0.5);
        }
        .joystick-thumb, .joystick-left, .joystick-right{
            &.selected{
                svg{
                    path{fill: orangered !important;}
                }
            }
        }
    }
}
</style>