<template>
    <div class="thrustmaster-t16000">
        <div class="thrustContainer" :v-if="thruster.visible">
            <Thruster v-bind:axes="thruster.axes" v-bind:buttons="thruster.buttons"></Thruster>
        </div>
        <div class="pedalsContainer" :v-if="pedals.visible">
            <Pedals v-bind:axes="pedals.axes"></Pedals>
        </div>
        <div class="joystickContainer" :v-if="joystick.visible">
            <Joystick v-bind:axes="joystick.axes" v-bind:buttons="joystick.buttons"></Joystick>
        </div>
    </div>
</template>

<script>
    import Joystick from './parts/Joystick.vue';
    import Thruster from './parts/Thruster.vue';
    import Pedals from './parts/Pedals.vue';
    const haveEvents = 'ongamepadconnected' in window;
    const rAF = window.mozRequestAnimationFrame || window.webkitRequestAnimationFrame || window.requestAnimationFrame;
    export default{
    data() {
        return {
            controllers: {},
        };
    },
    computed: {
        thruster() {
            let response = {
                axes: {},
                buttons: {},
                visible: false
            };
            Object.entries(this.controllers).forEach(gamepad => {
                if (gamepad[1].id == "TWCS Throttle (Vendor: 044f Product: b687)") {
                    response = {
                        axes: {
                            x: this.controllers[gamepad[1].index].axes[0],
                            y: this.controllers[gamepad[1].index].axes[1],
                            acceleration: this.controllers[gamepad[1].index].axes[2],
                            butterfly: this.controllers[gamepad[1].index].axes[5],
                            zoom: this.controllers[gamepad[1].index].axes[7],
                            pov1: this.controllers[gamepad[1].index].axes[9],
                        },
                        buttons: {
                            thumb: this.controllers[gamepad[1].index].buttons[0],
                            pinky: this.controllers[gamepad[1].index].buttons[1],
                            anelar: this.controllers[gamepad[1].index].buttons[2],
                            middleUp: this.controllers[gamepad[1].index].buttons[3],
                            middleDown: this.controllers[gamepad[1].index].buttons[4],
                            middlePress: this.controllers[gamepad[1].index].buttons[5],
                            centerUp: this.controllers[gamepad[1].index].buttons[6],
                            centerRight: this.controllers[gamepad[1].index].buttons[7],
                            centerDown: this.controllers[gamepad[1].index].buttons[8],
                            centerLeft: this.controllers[gamepad[1].index].buttons[9],
                            bottomUp: this.controllers[gamepad[1].index].buttons[10],
                            bottomRight: this.controllers[gamepad[1].index].buttons[11],
                            bottomDown: this.controllers[gamepad[1].index].buttons[12],
                            bottomLeft: this.controllers[gamepad[1].index].buttons[13],
                        },
                        visible: true
                    };
                }
            });
            return response;
        },
        pedals() {
            let response = {
                axes: {},
                buttons: {},
                visible: false
            };
            Object.entries(this.controllers).forEach(gamepad => {
                if (gamepad[1].id == "TWCS Throttle (Vendor: 044f Product: b687)") {
                    response = {
                        axes: {
                            rightPedal: this.controllers[gamepad[1].index].axes[3],
                            leftPedal: this.controllers[gamepad[1].index].axes[4],
                            pedalRoll: this.controllers[gamepad[1].index].axes[6],
                        },
                        visible: true
                    };
                }
            });
            return response;
        },
        joystick() {
            let response = {
                axes: {},
                buttons: {},
                visible: false
            };
            Object.entries(this.controllers).forEach(gamepad => {
                if (gamepad[1].id == "T.16000M (Vendor: 044f Product: b10a)") {
                    response = {
                        axes: {
                            x: this.controllers[gamepad[1].index].axes[0],
                            y: this.controllers[gamepad[1].index].axes[1],
                            twist: this.controllers[gamepad[1].index].axes[5],
                            scroll: this.controllers[gamepad[1].index].axes[6],
                            pov1: this.controllers[gamepad[1].index].axes[9],
                        },
                        buttons: {
                            button1: this.controllers[gamepad[1].index].buttons[0],
                            button2: this.controllers[gamepad[1].index].buttons[1],
                            button3: this.controllers[gamepad[1].index].buttons[2],
                            button4: this.controllers[gamepad[1].index].buttons[3],
                            button5: this.controllers[gamepad[1].index].buttons[4],
                            button6: this.controllers[gamepad[1].index].buttons[5],
                            button7: this.controllers[gamepad[1].index].buttons[6],
                            button8: this.controllers[gamepad[1].index].buttons[7],
                            button9: this.controllers[gamepad[1].index].buttons[8],
                            button10: this.controllers[gamepad[1].index].buttons[9],
                            button11: this.controllers[gamepad[1].index].buttons[10],
                            button12: this.controllers[gamepad[1].index].buttons[11],
                            button13: this.controllers[gamepad[1].index].buttons[12],
                            button14: this.controllers[gamepad[1].index].buttons[13],
                            button15: this.controllers[gamepad[1].index].buttons[14],
                            button16: this.controllers[gamepad[1].index].buttons[15],
                        },
                        visible: true
                    };
                }
            });
            return response;
        }
    },
    beforeMount() {
        window.addEventListener("gamepadconnected", this.connecthandler);
        window.addEventListener("gamepaddisconnected", this.removeGamepad);
        //setInterval(this.updatestatus, 500);
    },
    mounted() {
        rAF(this.updatestatus);
        window.controllers = this.controllers;
    },
    methods: {
        connecthandler(e) {
            this.addgamepad(e.gamepad);
        },
        addgamepad(gamepad) {
            if (gamepad.id === undefined)
                return;
            this.controllers[gamepad.index] = gamepad;
            if (gamepad.id == "TWCS Throttle (Vendor: 044f Product: b687)") {
                this.pedals.axes = {
                    rightPedal: this.controllers[gamepad.index].axes[3],
                    leftPedal: this.controllers[gamepad.index].axes[4],
                    pedalRoll: this.controllers[gamepad.index].axes[6],
                };
                this.pedals.visible = true;
            }
        },
        removeGamepad(gamepad) {
            delete this.controllers[gamepad.index];
        },
        scangamepads() {
            let gamepads = navigator.getGamepads ? navigator.getGamepads() : (navigator.webkitGetGamepads ? navigator.webkitGetGamepads() : []);
            Object.entries(gamepads).forEach((gamepad) => {
                if (gamepad[1]) {
                    if (gamepad[1].index in this.controllers) {
                        this.controllers[gamepad[1].index] = gamepad[1];
                    }
                    else
                        this.addgamepad(gamepad);
                }
            });
            rAF(this.updatestatus);
        },
        updatestatus() {
            this.scangamepads();
        }
    },
    components: { Pedals, Thruster, Joystick }
}
</script>

<style lang="scss">
    svg{
        max-width: 100%;
        display: block;
    }
    .thrustmaster-t16000, main{
        min-width: 100%;
        height: 100%;
        display: block;
    }
    .thrustContainer{
        position: absolute;
        top: 0;
        left: 0;
        padding: 1rem 0 0 1rem;
    }
    .pedalsContainer{
        position: absolute;
        bottom: 0;
        left: 0;
        padding: 0 0 1rem 1rem;
    }
    .joystickContainer{
        position: absolute;
        top: 0;
        right: 0;
        padding: 1rem 1rem 0 0;
    }
</style>