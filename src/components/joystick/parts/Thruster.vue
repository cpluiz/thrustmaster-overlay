<template>
    <div id="thruster" :v-if="initialized">
        <div class="thruster-bg">
            <ThrustBase></ThrustBase>
        </div>
        <div class="thruster-vertical">
            <div class="container">
                <div class="thruster-head" :style="{'--thrusterVertical': thrusterPosition}">
                    <div class="head-bg">
                        <ThrustHead></ThrustHead>
                    </div>
                    <div class="head-pov1">
                        <CircleGroup v-bind:pov1="axes.pov1"></CircleGroup>
                    </div>
                    <div class="head-middle">
                        <CircleGroup v-bind:buttons="{up:btns.centerUp, right: btns.centerRight, down: btns.centerDown, left: btns.centerLeft}"></CircleGroup>
                    </div>
                    <div class="head-bottom">
                        <div class="button-holder">
                            <CircleGroup v-bind:buttons="{up:btns.bottomUp, right: btns.bottomRight, down: btns.bottomDown, left: btns.bottomLeft}"></CircleGroup>
                        </div>
                    </div>
                    <div class="head-flip" :style="{'--flipRoptation': flipRotation}">
                        <div class="container">
                            <ThrusterFlip></ThrusterFlip>
                        </div>
                    </div>
                    <div class="push-button pink-finger">
                        <div class="button-container">
                            <ThrusterButton v-bind:selected="btns.pinky !== undefined && btns.pinky.pressed"></ThrusterButton>
                        </div>
                    </div>
                    <div class="push-button anelar-finger">
                        <div class="button-container">
                            <ThrusterButton v-bind:selected="btns.anelar !== undefined && btns.anelar.pressed"></ThrusterButton>
                        </div>
                    </div>
                    <div class="push-button thumb-finger">
                        <div class="button-container">
                            <ThrusterButton v-bind:selected="btns.thumb !== undefined && btns.thumb.pressed"></ThrusterButton>
                        </div>
                    </div>
                    <div class="analog-block">
                        <div class="analog">
                            <div class="middle-switch" :style="{'--buttonPosition' : middleFingerPosition}">
                            </div>
                        </div>
                        <div class="thruster-analog">
                            <AnalogPad v-bind:x="axs.x" v-bind:y="axs.y" v-bind:pressed="middlePress"></AnalogPad>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import ThrustBase from './assets/ThrustBase.vue';
    import ThrustHead from './assets/ThrustHead.vue';
    import ThrusterB2 from './assets/ThrusterB2.vue';
    import ThrusterB1 from './assets/ThrusterB1.vue';
    import ThrusterFlip from './assets/ThrusterFlip.vue';
    import ThrusterButton from './assets/ThrusterButton.vue';
    import AnalogPad from './subparts/AnalogPad.vue';
import CircleGroup from './subparts/CircleGroup.vue';
    export default{
    props: ["buttons", "axes"],
    data() {
        return {
            data: null
        };
    },
    computed:{
        thrusterPosition(){
            return (this.axes.acceleration  * 50)+"%";
        },
        btns(){
            return this.buttons;
        },
        axs(){
            return this.axes;
        },
        initialized(){
            return Object.entries(this.buttons).length > 0;
        },
        flipRotation(){
            return ((this.axes.butterfly * 10)-15)+'deg';
        },
        middleFingerPosition(){
            return this.btns.middleUp != undefined ? this.buttons.middleUp.pressed ? '-100%' : this.buttons.middleDown.pressed ? '100%' : 0 : 0;
        },
        middlePress(){
            return this.btns.middlePress !== undefined && this.btns.middlePress.pressed;
        }
    },
    mounted(){
    },
    methods: {},
    components: { ThrustBase, ThrustHead, ThrusterFlip, ThrusterButton, AnalogPad, CircleGroup }
}
</script>

<style lang="scss">
    #thruster{
        position: relative;
        display: block;
        width:202px;
        height: 267px;
        .thruster-bg{
            position: absolute;
            top: 0;
            left: 0;
        }
        .thruster-vertical{
            position: absolute;
            width: 174px;
            height: 160px;
            left: 50%;
            top:calc(50% + 10px);
            transform: translate(-50%, -50%);
            .container{
                position: relative;
                width: 100%;
                height: 100%;
                .thruster-head{
                    position: absolute;
                    top: 0;
                    left: 50%;
                    width: 100%;
                    transform: translate(-50%, var(--thrusterVertical));
                }
            }
        }
        .thruster-head{
            z-index:5;
            .head-pov1{
                position: absolute;
                top: 0.8rem;
                right: -0.2rem;
                transform: scale(0.45, 0.5);
            }
            .head-middle{
                position: absolute;
                top: 1.95rem;
                right: 0.3rem;
                transform: scale(0.45, 0.5);
            }
            .head-bottom{
                position: absolute;
                top: 2.9rem;
                right: -0.3rem;
                transform: scale(0.45, 0.5);
            }
            .head-flip{
                position: absolute;
                top: -2px;
                left: 33px;
                transform: rotate(var(--flipRoptation));
                .container{
                    position: relative;
                    width: 84px;
                    height: 13px;
                    display: block;
                    transform: transition(-50%, 0)
                }
            }
            .push-button{
                position: absolute;
                z-index: -1;
                .button-container{
                    padding-top: 0;
                    transition: all ease-in-out 0.15s;
                }
                &.selected{
                    .button-container{
                        padding-top: 3px;
                    }
                }
                &.pink-finger{
                    top: 27px;
                    left: 18px;
                    transform: rotate(-25deg);
                }
                &.anelar-finger{
                    top: 19px;
                    left: 33px;
                    transform: rotate(-25deg);
                }
                &.thumb-finger{
                    bottom: 1.7rem;
                    right: 0.8rem;
                    transform: rotate(110deg);
                    z-index: 100;
                }
            }
            .analog-block{
                position: relative;
                .analog{
                    height: 80px;
                    position: absolute;
                    left: 3.9rem;
                    top: -3.2rem;
                    .middle-switch{
                        transform: translateY(-50%);
                        height: 2rem;
                        width: 10px;
                        border: 2px solid #232323;
                        background-color: rgba(116, 116, 116, 0.7);
                        &:before{
                            display: block;
                            width: 100%;
                            height: 33%;
                            background-color: #232323;
                            content: '';
                            position: absolute;
                            top:calc(50% - 0.29rem);
                            transform: translateY(var(--buttonPosition))
                        }
                    }
                }
                .thruster-analog{
                    position: absolute;
                    top: -4.2rem;
                    left: 5.2rem;
                    width: 2rem;
                    height: 2rem;
                    .analog-container{
                        background-color: rgba(116, 116, 116, 0.7);
                        border: 2px solid #232323;
                    }
                }
            }
        }
    }
</style>