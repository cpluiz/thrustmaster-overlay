<template>
    <div class="button-container" :class="{'selected':selected}">
        <svg width="8" height="6" viewBox="0 0 8 6" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M6.70945 4.6502C4.80785 4.33309 3.5763 4.34483 1.11328 4.6576L1.10955 1.83386C2.09524 0.42069 5.38649 -0.0542835 6.70572 1.82647L6.70945 4.6502Z" fill="#F35A04" stroke="#FF4D00"/>
        </svg>
    </div>
</template>
<script>
export default{
    props:['selected']
}
</script>
<style lang="scss">
.button-container{
    display: block;
    width: 8px;
    height: 6px;
    position: relative;
    padding-top: 0;
    transition: all ease-in-out 0.15s;
    overflow: hidden;
    .svg{
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
    }
    &.selected{
        height: 3px;
        margin-top: 3px;
    }
}
</style>