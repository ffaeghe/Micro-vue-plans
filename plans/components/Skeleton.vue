<template>
    <div :class="('sk')">
        <div :class="('sk__head')">
            <div :class="className('sk__content', 'title')" />
            <div :class="className('sk__content', 'text')" />
        </div>
        <div :class="className('sk__body')">
            <div :class="className('sk__plan')">
                <div :class="className('sk__content', 'icon')" />
                <div :class="className('sk__content', 'title')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
            </div>
            <div :class="className('sk__plan')">
                <div :class="className('sk__content', 'icon')" />
                <div :class="className('sk__content', 'title')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
            </div>
            <div :class="className('sk__plan')">
                <div :class="className('sk__content', 'icon')" />
                <div :class="className('sk__content', 'title')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
            </div>
            <div :class="className('sk__plan')">
                <div :class="className('sk__content', 'icon')" />
                <div :class="className('sk__content', 'title')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
                <div :class="className('sk__content', 'text')" />
            </div>
        </div>
    </div>
</template>
<script>
import { numberProp } from '@panel/core-modules/modules/propTypes';
/*  eslint {indent: ['error', 4],
    semi: ['error', 'always'],
    comma-dangle: ['error', 'always-multiline']}
*/
import className from '../className';

export default {
    name: 'ActivePlanOverview',
    mixins: [className],
    props: {
        plan: numberProp(false, 1),
    },
    methods: {
        redirect() {
            this.$emit('redirect');
        },
    },
};
</script>
