<template>
  <div class="ck-cp-menu">
    <div class="ck-cp-controller-bar">
      <button type="button" class="cp-btn" @click="handleChangePickerMode('solid')"
        :class="{ 'cp-btn-active': props.mode == 'solid' }"
      >
        <span class="ck-cp-svg-button-wrapper">
          <svg width="32" height="32" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="0.5" y="0.5" width="31" height="31" rx="11.5" fill="transparent" 
              :stroke="props.mode == 'solid' ? '#C09FE9' : '#F3F3F3'"
            />
            <rect x="4.5" y="4.5" width="23" height="23" rx="11.5" fill="#C09FE9" stroke="#C09FE9"/>
          </svg>
        </span>
      </button>
      
      <button type="button" class="cp-btn" @click="handleChangePickerMode('gradient-linear')"
        :class="{ 'cp-btn-active': props.mode == 'gradient' && props.gradientType == 'linear' }"
      >
        <span class="ck-cp-svg-button-wrapper">
          <svg width="32" height="32" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="0.5" y="0.5" width="31" height="31" rx="11.5" 
              :stroke="props.mode == 'gradient' && props.gradientType == 'linear' ? '#C09FE9' : '#F3F3F3'"
            />
            <rect x="4" y="4" width="24" height="24" rx="12" fill="url(#paint0_linear_660_191)"/>
            <defs>
              <linearGradient id="paint0_linear_660_191" x1="4" y1="16" x2="28" y2="16" gradientUnits="userSpaceOnUse">
                <stop offset="0.293269" stop-color="#C09FE9"/>
                <stop offset="1" stop-color="white"/>
              </linearGradient>
            </defs>
          </svg>
        </span>
      </button>
      
      <button type="button" class="cp-btn" @click="handleChangePickerMode('gradient-radial')"
        :class="{ 'cp-btn-active': props.mode == 'gradient' && props.gradientType == 'radial' }"
      >
        <span class="ck-cp-svg-button-wrapper">
          <svg width="32" height="32" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="0.5" y="0.5" width="31" height="31" rx="11.5" 
              :stroke="props.mode == 'gradient' && props.gradientType == 'radial' ? '#C09FE9' : '#F3F3F3'"
            />
            <rect x="4" y="4" width="24" height="24" rx="12" fill="url(#paint0_radial_660_191)"/>
            <defs>
              <radialGradient id="paint0_radial_660_191" cx="0" cy="0" r="1" gradientUnits="userSpaceOnUse" gradientTransform="translate(16 16) scale(12)">
                <stop offset="0.543269" stop-color="#C09FE9"/>
                <stop offset="1" stop-color="white"/>
              </radialGradient>
            </defs>
          </svg>
        </span>
      </button>
      

      <ColorInputMenu v-if="showInputMenu" style="margin-bottom: var(--margin-top)" :iconClass="iconClasses.arrowDown"
        :inputType="inputType" @onChangeInputName="handleChangeInputType">
      </ColorInputMenu>

    </div>

    <div v-if="mode == 'gradient'" v-show="gradientType == 'linear'"
      class="ck-cp-linear-angle-container"
    >
       <p class="ck-gradient-set-label" style="margin-top: 0">
        Угол наклона
      </p>
      <input type="number" min="0" max="360" :value="angle" @input="handleInput($event, 'angle')" />
    </div>

    <div v-if="mode == 'gradient'" v-show="gradientType == 'radial'"
      class="ck-cp-linear-angle-container"
    >
      <p class="ck-gradient-set-label" style="margin-top: 0">
        Ось X | Y
      </p>
      <input type="number" min="0" max="100" :value="percentageX" @input="handleInput($event, 'percentageX')" />
      <input type="number" min="0" max="100" :value="percentageY" @input="handleInput($event, 'percentageY')" />
    </div>
  </div>
  
</template>

<script setup lang="ts">
import { Mode, InputType, Local, IconClasses } from "../core/types/types.ts";
import ColorInputMenu from "./ColorInputMenu.vue";

const props = defineProps({
  mode: {
    default: "gradient",
    type: String as () => Mode,
  },
  inputType: { default: "RGB", type: String as () => InputType },
  gradientType: { default: "linear", type: String },
  showInputMenu: { default: true, type: Boolean },
  showPickerMode: { default: true, type: Boolean },
  angle: { default: 90, type: Number },
  percentageX: { default: 50, type: Number },
  percentageY: { default: 50, type: Number },
  local: {
    default: {},
    type: Object as () => Local,
  },
  iconClasses: {
    default: {
      ruler: "",
      eyeDroper: "",
      arrowDown: "",
      save: "",
      delete: "",
    },
    type: Object as () => IconClasses,
  },
});


const emits = defineEmits<{
  (e: "onPickerChangeMode", value: Mode): void;
  (e: "onChangeMode", value: string): void;
  (e: "onChangeInputType", value: InputType): void;
  (e: "onDeleteColor"): void;
  (e: "onClickEyeDropper"): void;
  (e: "update:angle", value: number): void;
  (e: "update:percentageX", value: number): void;
  (e: "update:percentageY", value: number): void;
  (e: "onInput"): void;
  (e: "onChangeGlobalMode", value: Array<string>): void;
}>();

const handleInput = (event: Event, type: string) => {
  switch (type) {
    case "angle":
      emits("update:angle", parseInt((event.target as HTMLInputElement).value));
      emits("onInput");
      break;

    case "percentageX":
      emits(
        "update:percentageX",
        parseInt((event.target as HTMLInputElement).value)
      );
      emits("onInput");
      break;
    case "percentageY":
      emits(
        "update:percentageY",
        parseInt((event.target as HTMLInputElement).value)
      );
      emits("onInput");
      break;
  }
};

const handleChangeInputType = (event: InputType) => {
  emits("onChangeInputType", event);
};
/*
const handleChangeGradientMode = ( ) => {
  emits("onChangeMode", props.gradientType == "linear" ? "radial" : "linear");
};
*/

const handleChangePickerMode = ( type: string ) => {
  props.mode;
  console.log(type)
  switch (type) {
    case "gradient-linear":
      emits("onChangeGlobalMode", ["gradient", "linear"]);
      //emits("onChangeMode", "linear");
      break;
    case "gradient-radial":
      emits("onChangeGlobalMode", ["gradient", "radial"]);
      //emits("onChangeMode", "radial");
      break;
    case "solid":
      emits("onChangeGlobalMode", ["solid"]);
      break;
    default:
      break;
  }
};
</script>

<style lang="scss" scoped></style>
