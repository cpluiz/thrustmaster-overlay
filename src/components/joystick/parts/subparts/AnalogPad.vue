<template>
    <div class="analog-container">
        <div class="analog-pad" :style="{'--x':axisX,'--y':axisY}" :class="{'active':isPressed}">
            <slot></slot>
        </div>
    </div>
</template>

<script>
    export default{
        props: ['x','y','pressed'],
        data(){
            return{
                data: null
            }
        },
        computed:{
            axisX(){
                if(this.x === undefined) return '50%';
                return 'calc('+(this.x*50 + 50)+'% + '+this.x*-0.25+'rem)';
            },
            axisY(){
                if(this.y === undefined) return '50%';
                return 'calc('+(this.y*50 + 50)+'% + '+this.y*-0.25+'rem)';
            },
            isPressed(){
                return this.pressed != undefined ? this.pressed : false;
            }
        }
    }
</script>

<style lang="scss">
.analog-container{
    position: relative;
    width: 100%;
    height: 100%;
    .analog-pad{
        position: absolute;
        overflow: visible;
        width:1px; height: 1px;
        left: var(--x);
        top: var(--y);
        &:before{
            display: block;
            content: '';
            transform: translate(-50%, -50%);
            width: 0.5rem;
            height: 0.5rem;
            border: 2px solid #232323;
            border-radius: 100rem;
            background-color: #232323;
        }
        &.active{
            &:before{background-color: #f35a04}
        }
    }
}
</style>