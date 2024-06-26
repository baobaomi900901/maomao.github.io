<template>
  <div :class="{ 'button-loading': props.loading }" class="k-button">
    <el-button
      ref="buttonRef"
      class="k-button__inner"
      :class="{ loading: props.loading }"
      v-bind="attrs"
      @click="handleClick"
    >
      <slot name="iconLeft" class="icon-left">
        <span v-if="props.iconLeft">
          <props.iconLeft class="icon-left" />
        </span>
      </slot>

      <span v-if="props.value">{{ props.value }}</span>
      <label v-else><slot class="slot-content"></slot></label>
      <slot name="iconRight" class="icon-right">
        <span v-if="props.iconRight">
          <props.iconRight class="icon-right" />
        </span>
      </slot>
    </el-button>
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";
import { IButtonProps } from "../../interface/index";
import { IconLoading } from "ksw-vue-icon";
import { log } from "console";
import "ksw-vue-icon/styles/icon.css";

defineOptions({
  name: "KButton",
});

const props = withDefaults(defineProps<IButtonProps>(), {
  type: "",
  size: "base",
  value: "",
  disabled: false,
  loading: false,
  loadingIcon: IconLoading,
});

console.log(props.loadingIcon.props.size);

props.loadingIcon.props.spin = {
  type: Boolean,
  default: true,
};
props.loadingIcon.props.color = {
  type: String,
  default: "#2882ff", //currentColor
};

const attrs = computed(() => {
  const { type, size } = props;
  return {
    ...getBtnTypeAttrs(type),
    ...getSizeAttrs(size),
    ...getOriginAttrs(),
  };
});

const getBtnTypeAttrs = (type: string): object => {
  const typeAttrs = {
    type: "",
    plain: false,
    text: false,
    iconLeft: null,
    iconRight: null,
  };
  switch (type) {
    case "":
      typeAttrs.type = "";
      break;
    case "main":
      typeAttrs.type = "main";
      break;
    case "secondary":
      typeAttrs.type = "secondary";
      break;
    case "text":
      typeAttrs.type = "text";
      break;
    case "icon":
      typeAttrs.type = "icon";
      break;
  }
  return typeAttrs;
};

const getSizeAttrs = (size: string): object => {
  const sizeAttrs = {
    size: "default",
  };
  switch (size) {
    case "base":
      sizeAttrs.size = "";
      break;
    case "sm":
      sizeAttrs.size = "sm";
      break;
  }
  return sizeAttrs;
};

const getOriginAttrs = () => {
  const { disabled, loading, loadingIcon } = props;
  return {
    disabled,
    loading,
    loadingIcon,
  };
};

const emits = defineEmits(["click"]);
const handleClick = (e: Event) => {
  emits("click", e);
};
</script>

<style lang="less">
@import "./style.less";
</style>