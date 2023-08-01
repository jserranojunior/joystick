<template>
  <div class="py-12 text-red-900 flex justify-center">
    <div class="flex border border-gray-200 pl-8 rounded-lg">
      <div class="analog-stick my-auto">
        <div id="joystick-handle" :style="{ transform: `translate(${joystickPosition.x}px, ${joystickPosition.y}px)` }">
        </div>
      </div>
      <div class="pl-2 pr-4 py-4 ">
        <div class="flex flex-wrap justify-end mb-2">
          <div class="mx-1 border border-gray-100 p-4 rounded-full" id="button-9"
            :class="{ 'active': buttons[8].isActive }"></div>
          <div class="mx-1 border border-gray-100 p-4 rounded-full" id="button-10"
            :class="{ 'active': buttons[9].isActive }"></div>

        </div>
        <div class="flex flex-wrap ">
          <div class="mt-6">
            <div class="mx-1 border border-gray-100 p-8 rounded-full" id="button-0"
            :class="{ 'active': buttons[0].isActive }"></div>
          </div>
          <div class="mt-2">
            <div class="mx-1 border border-gray-100 p-8 rounded-full" id="button-3"
              :class="{ 'active': buttons[3].isActive }"></div>
          </div>
          <div class="mt-3">
            <div class="mx-1 border border-gray-100 p-8 rounded-full" id="button-0"
              :class="{ 'active': buttons[6].isActive }"></div>
          </div>
          <div class="mt-6">
            <div class="mx-1 border border-gray-100 p-8 rounded-full" id="button-2"
              :class="{ 'active': buttons[4].isActive }"></div>
          </div>


        </div>
        <div class="flex flex-wrap">
          <div class="mt-2">
          
            <div class="mx-1  border border-gray-100 p-8 rounded-full" id="button-2"
              :class="{ 'active': buttons[2].isActive }"></div>
          </div>
          <div class="">
          <div class="mx-1 border border-gray-100 p-8 rounded-full" id="button-1"
            :class="{ 'active': buttons[1].isActive }"></div>
            </div>
            <div class="">
          <div class="mx-1 border border-gray-100 p-8 rounded-full" id="button-1"
            :class="{ 'active': buttons[7].isActive }"></div>
            </div>
            <div class="mt-2">
          <div class="mx-1 border border-gray-100 p-8 rounded-full" id="button-3"
            :class="{ 'active': buttons[5].isActive }"></div>
            </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";

const buttons = ref([
  { id: "button-0", label: "1", isActive: false },
  { id: "button-1", label: "2", isActive: false },
  { id: "button-2", label: "3", isActive: false },
  { id: "button-3", label: "4", isActive: false },
  { id: "button-4", label: "5", isActive: false },
  { id: "button-5", label: "6", isActive: false },
  { id: "button-6", label: "7", isActive: false },
  { id: "button-7", label: "8", isActive: false },
  { id: "button-8", label: "9", isActive: false },
  { id: "button-9", label: "10", isActive: false },
]);

const joystickPosition = ref({ x: 0, y: 0 });
const joystickThreshold = 0.2;
const joystickMaxOffset = 25;

onMounted(() => {
  updateGamepadState();
  updateJoystickPosition();
});

function updateGamepadState() {
  const handleGamepadState = () => {
    const gamepads = navigator.getGamepads();

    for (let i = 0; i < gamepads.length; i++) {
      const gamepad = gamepads[i];

      if (gamepad && gamepad.connected) {
        const buttonsState = gamepad.buttons;

        buttons.value.forEach((button, index) => {
          button.isActive = buttonsState[index].pressed;
        });
      }
    }

    requestAnimationFrame(handleGamepadState);
  };

  handleGamepadState();
}

function updateJoystickPosition() {
  const handleJoystickPosition = () => {
    const gamepads = navigator.getGamepads();
    const joystick = gamepads[0];

    if (joystick && joystick.connected) {
      const xAxis = joystick.axes[0];
      const yAxis = joystick.axes[1];

      const threshold = joystickThreshold;
      const maxOffset = joystickMaxOffset;
      const positionX = Math.abs(xAxis) > threshold ? xAxis * maxOffset : 0;
      const positionY = Math.abs(yAxis) > threshold ? yAxis * maxOffset : 0;

      joystickPosition.value = { x: positionX, y: positionY };
    }

    requestAnimationFrame(handleJoystickPosition);
  };

  handleJoystickPosition();
}
</script>

<style scoped>
#arcade-stick {
  text-align: center;
  background-color: #333;
  padding: 20px;
}

#arcade {
  display: flex;
  justify-content: center;
  align-items: center;
}

.analog-stick {
  width: 80px;
  height: 80px;
  background-color: #777;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-right: 20px;
}

#joystick-handle {
  width: 80px;
  height: 80px;
  background-color: #ff0000;
  border-radius: 50%;
  transform: translate(0, 0);
}

.arcade-buttons {
  display: flex;
  flex-wrap: wrap;
}

.arcade-btn {
  width: 60px;
  height: 60px;
  background-color: #000;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 14px;
  color: #fff;
  margin: 5px;
  cursor: pointer;
  user-select: none;
  border: 2px solid #fff;
}

.active {
  background-color: #ff0000;
  box-shadow: 0 0 10px #ff0000;
}

.arcade-btn.active {
  background-color: #ff0000;
  box-shadow: 0 0 10px #ff0000;
}

/* Estilos adicionais para um design gamer */
.arcade-btn:before {
  content: '';
  position: absolute;
  width: 10px;
  height: 10px;
  background-color: #fff;
  border-radius: 50%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.arcade-btn:hover:before {
  width: 20px;
  height: 20px;
}

.arcade-btn:active:before {
  width: 15px;
  height: 15px;
}</style>