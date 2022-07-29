<template>
    <div class="pedalArea" v-if="visible">
        <div class="basePedal">
            <BasePedal></BasePedal>
        </div>
        <div class="pedalPivot" :style="{'--angle': angle}">
            <div class="container">
                <div class="pivotBackground">
                    <PedalPivot></PedalPivot>
                </div>
                <div class="leftPedal actionPedal" :style="{'--pivotAngle': inverseAngle, '--actionAngle': angleLeftPedal}">
                
                    <Pedal></Pedal>
                </div>
                <div class="rightPedal actionPedal" :style="{'--pivotAngle': inverseAngle, '--actionAngle': angleRightPedal}">
                    <Pedal></Pedal>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import BasePedal from './assets/BasePedal.vue';
    import PedalPivot from './assets/PedalPivot.vue';
    import Pedal from './assets/Pedal.vue';
    export default{
    props: ['axes'],
    data() {
        return {
            timestamp: 0
        }
    },
    computed:{
        angle(){
            return (this.axes.pedalRoll * -45)+"deg"
        },
        inverseAngle(){
            return (this.axes.pedalRoll * 45)+"deg"
        },
        angleLeftPedal(){
            return (this.axes.leftPedal * 30)+"deg"
        },
        angleRightPedal(){
            return (this.axes.rightPedal * 30)+"deg"
        },
        show(){
            return this.axes.pedalRoll < -0.1 || this.axes.pedalRoll > 0.1  || this.axes.leftPedal < 0.9 || this.axes.rightPedal < 0.9;
        },
        visible(){
            return this.show || this.timestamp > 0;
        },
    },
    beforeMount(){
        window.test = this;
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
    components: { BasePedal, PedalPivot, Pedal }
}
</script>

<style lang="scss">
    .pedalArea{
        position:relative;
        width:  267px;
        height: 202px;
        .basePedal{
            position: absolute;
            top: 0;
            left: 50%;
            width: 100%;
            height: 100%;
            transform: translateX(-50%)
        }
        .pedalPivot{
            position:absolute;
            top: 50%;
            left: 50%;
            width: 70%;
            transform: translate(-50%, -50%) rotateZ(var(--angle));
            .container{
                position: relative;
                height: 95px;
                width: 100%;
            }
        }
        .pivotBackground{
            position: absolute;
            left: 50%;
            top: 65%;
            width: 100%;
            transform: translate(-50%, -50%);
        }
        .actionPedal{
            position: absolute;
            top: 50%;
            padding-bottom: 1rem;
            &.leftPedal{
                left: 0;
                transform: translate(-25%, -50%) rotateZ(var(--pivotAngle)) rotateX(var(--actionAngle));
            }
            &.rightPedal{
                right: 0;
                transform: translate(25%, -50%) rotateZ(var(--pivotAngle)) rotateX(var(--actionAngle));
            }
        }
        svg{
            width: 100%;
            height: 100%;
        }
    }
</style>