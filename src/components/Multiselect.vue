<template>
  <div class="multiselect" :class="selectStyle">
    <input
      class="multiselect__input opacity-0 cursor-default"
      :class="inputStyle"
      @blur="onBlur"
      @click="openOptions"
      @input="inputChange"
      ref="input"
      v-scroll-stop
    />
    <div dir="auto" class="multiselect__tags">
      <input
        class="
          cursor-default
          opacity-0
          absolute
          left-0
          right-0
          top-0
          bottom-0
          appearance-none
          w-full
          h-full
        "
        @click="openOptions"
      />
      <div
        v-for="(option, i) in selectedOptions"
        :key="i"
        class="multiselect__tag"
        :class="tagStyle"
      >
        {{ getOptionName(option) }}
        <span class="multiselect__tag-icon" @click="removeTag(option)"></span>
      </div>
    </div>
    <div
      class="
        textinput-icon
        absolute
        duration-300
        end-0
        top-0
        bottom-0
        mt-1
        me-2
        pointer-events-none
        self-center
        flex
        items-center
        justify-center
      "
    >
      <MyIcon name="arrow-down" class="w-8 h-8 me-5" />
    </div>

    <div
      v-if="openSelect"
      class="multiselect__options"
      @mouseover="optionClicked = true"
      @mouseenter="optionClicked = true"
      @mouseleave="optionClicked = false"
    >
      <div
        dir="auto"
        v-for="(option, i) in remainOptions"
        :key="i"
        class="multiselect__option"
        :class="optionStyle"
        @click="addTag(option)"
      >
        {{ getOptionName(option) }}
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import MyIcon from "./MyIcon.vue";

import Vue from "vue";
export default Vue.extend({
  model: {
    prop: "value",
    event: "change",
  },
  components: {
    MyIcon,
  },
  data() {
    return {
      openSelect: false,
      optionClicked: false,
    };
  },
  props: {
    selectStyle: String,
    inputStyle: String,
    tagStyle: String,
    optionStyle: String,
    options: {
      default: () => [],
      type: Array,
    },
    optionName: {
      default: null,
      type: [String, Number],
    },
    optionValue: {
      default: null,
      type: [String, Number],
    },
    value: {
      default: () => [],
      type: Array,
    },
  },
  computed: {
    remainOptions(): any[] {
      return this.options?.filter(
        (o: any) => !this.value.includes(this.getOptionValue(o))
      );
    },
    selectedOptions(): any[] {
      return this.value?.map((v) => {
        return (
          this.options?.find((o: any) => this.getOptionValue(o) == v) || {}
        );
      });
    },
  },
  methods: {
    getOptionName(option: any) {
      return this.optionName ? option[this.optionName] : option;
    },
    getOptionValue(option: any) {
      return this.optionValue ? option[this.optionValue] : option;
    },
    openOptions() {
      this.openSelect = !this.openSelect;
    },
    addTag(option: any) {
      this.$emit(
        "change",
        [...this.value, this.getOptionValue(option)].filter((t1) =>
          this.options?.find((t2: any) => this.getOptionValue(t2) == t1)
        )
      );
      this.openSelect = false;
    },
    removeTag(option: any) {
      this.$emit(
        "change",
        this.value.filter((v) => v != this.getOptionValue(option))
      );
    },
    onBlur() {
      if (!this.optionClicked) this.openSelect = false;
      this.optionClicked = false;
    },
    inputChange(e: any) {
      e.target.value = "";
    },
  },
});
</script>

<style lang="scss" scoped>
.me-2 {
  margin-inline-start: 1rem;
}

[dir="rtl"] .end-0 {
  left: 0;
}

[dir="ltr"] .end-0 {
  right: 0;
}

.multiselect {
  position: relative;
  border-width: 1px;
  border-color: slategray;
  border-radius: 0.5rem;
  &__input {
    min-height: 36px;
    width: 100%;
    margin: 0;
    cursor: default;
  }

  &__tags {
    top: 0;
    bottom: 0;
    margin: 4px 0 4px 4rem;
    position: absolute;
    z-index: 100;
    overflow-y: auto;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    padding: 0.5rem 0.75rem 0.5rem 0;
    width: 100%;
  }

  &__tag {
    background-color: #ff000f;
    position: relative;
    display: inline-block;
    padding: 0.25rem 1.5rem 0.25rem 0.625rem;
    margin: 0.35rem 0.7rem 0.35rem 0;
    border-radius: 5px;
    color: #fff;
    line-height: 1;
    white-space: nowrap;
    overflow: hidden;
    max-width: 100%;
    text-overflow: ellipsis;
    cursor: default;
    flex-shrink: 0;

    &-icon {
      cursor: pointer;
      margin-left: 7px;
      position: absolute;
      right: 0;
      top: 8%;
      bottom: 0;
      font-weight: 700;
      font-style: normal;
      width: 22px;
      text-align: center;
      line-height: 22px;
      transition: all 0.2s ease;
      border-radius: 5px;

      &:after {
        content: "\D7";
        color: #266d4d;
        font-size: 0.875rem;
      }
    }
  }

  &__options {
    width: 100%;
    z-index: 100;
    position: absolute;
    border: solid 1px #90a4ae;
    background: white;
    min-height: 64px;
    max-height: 260px;
    overflow-y: auto;
    top: 100%;
    color: black;
  }

  &__option {
    cursor: default;
    width: 100%;
    padding: 0.1rem 1rem;
    &:hover {
      background: #90a4ae;
    }
  }
}

.textinput-icon {
  color: #263238;
}
</style>
