<template>
  <div
    v-show="locales.length > 1"
    class="nova-translatable-locale-tabs flex md:flex-row select-none"
    :class="{ 'px-8': !this.detail }"
  >
    <div :class="listClasses" v-if="displayType != 'none'">
      <a
        v-for="locale in locales"
        :key="locale.key"
        class="locale-tag ml-3 cursor-pointer font-bold text-80 text-sm"
        :class="{
          '-active': locale.key === activeLocale,
          '-error': hasError(locale.key),
        }"
        @click="() => $emit('tabClick', locale.key)"
        @dblclick="() => $emit('doubleClick', locale.key)"
      >
        {{ locale.name }}
      </a>
    </div>
  </div>
</template>

<script>
export default {
  props: ['locales', 'activeLocale', 'displayType', 'detail', 'errors', 'errorAttributes', 'localesWithErrors'],
  computed: {
    listClasses() {
      if (this.displayType === 'column') return ['ml-auto','flex', 'flex-col'];
      if (this.displayType === 'row-left') return ['mt-1 md:mt-0 pb-5 px-6 md:px-8 w-full md:inset-x-1/5 md:w-3/5 md:py-5'];
      return ['ml-auto'];
    },
  },

  methods: {
    hasError(locale) {
      if (Array.isArray(this.localesWithErrors) && this.localesWithErrors.includes(locale)) return true;
      if (!this.errors || !this.errorAttributes) return false;

      const errorAttribute = this.errorAttributes[locale];
      return this.errors.has(errorAttribute);
    },
  },
};
</script>

<style lang="scss">
.nova-translatable-locale-tabs {
  position: relative;
  z-index: 2;
  padding-top: 0.25rem;

  .locale-tag {
    border-bottom: 2px solid transparent;

    &.-active {
      color: rgba(var(--colors-primary-500));
      border-color: rgba(var(--colors-primary-500));
    }

    &.-error {
      color: rgba(var(--colors-red-500));
      border-color: rgba(var(--colors-red-500));

      &.-active {
        color: rgba(var(--colors-primary-500));
      }
    }
  }
}
</style>
