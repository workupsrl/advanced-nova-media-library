<template>
    <div class="advanced-nova-media-library">
        <PanelItem :index="index" :field="field">
            <template #value>
                <a
                    v-if="field.translatable"
                    class="inline-block font-bold cursor-pointer mr-2 animate-text-color select-none"
                    :class="{ 'text-60': localeKey !== currentLocale, 'text-primary': localeKey === currentLocale }"
                    :key="`a-${localeKey}`"
                    v-for="(locale, localeKey) in field.locales"
                    @click="changeTab(localeKey)"
                >
                    {{ locale }}
                </a>
                <div class="flex flex-row items-center">
                    <gallery v-model="gallery" :field="field" :multiple="field.multiple && !field.single"/>
                </div>
            </template>
        </PanelItem>
    </div>
</template>

<script>
import Gallery from '../Gallery';

export default {
    components: {
        Gallery,
    },
    props: ['index', 'resource', 'resourceName', 'resourceId', 'field'],
    data() {
        return {
            currentLocale: Object.keys(this.field?.locales ?? {})?.[0],
            gallery: [],
        }
    },
    computed: {
        gallery() {
            return this.field.value.filter(media => media.custom_properties.locale === this.currentLocale)
        }
    },
    methods: {
        changeTab(locale) {
            if (this.currentLocale !== locale) {
                Nova.$emit('localeChanged', locale);
                this.currentLocale = locale;
            }
        },
    },
};
</script>
