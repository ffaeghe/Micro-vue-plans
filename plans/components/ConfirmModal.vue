<template>
    <ar-modal
        :show="show"
        @close="onClose"
    >
        <div :class="className('confirmModal__body')">
            <div :class="className('confirmModal__icon')">
                <plan-icon :plan="plan" />
            </div>
            <i18n
                :class="className('confirmModal__message')"
                path="support.confirmModalMessage"
                tag="div"
            >
                <span :class="className('confirmModal__plan')">
                    {{ $t(`support.newPlans.${plan}.name`) }}
                </span>
            </i18n>
        </div>
        <div :class="className('confirmModal__actions')">
            <template v-if="loading">
                <ar-spinner
                    variant="primary"
                    sm
                />
            </template>
            <template v-else>
                <ar-button
                    :class="className('confirmModal__button', 'cancel')"
                    :disabled="loading"
                    @click="onClose"
                >
                    {{ $t('support.cancel') }}
                </ar-button>
                <ar-button
                    :class="className('confirmModal__button')"
                    :disabled="loading"
                    variant="primary"
                    @click="onConfirm"
                >
                    {{ $t('support.confirm') }}
                </ar-button>
            </template>
        </div>
    </ar-modal>
</template>
<script>
/*  eslint {indent: ['error', 4],
    semi: ['error', 'always'],
    comma-dangle: ['error', 'always-multiline']}
*/
import PlanIcon from '@arvan/support/components/PlanIcon';
import { booleanProp, numberProp } from '@panel/core-modules/modules/propTypes';
import className from '../className';

export default {
    name: 'ConfirmActivatePlan',
    components: {
        PlanIcon,
    },
    mixins: [className],
    props: {
        show: booleanProp(true, false),
        loading: booleanProp(false, false),
        plan: numberProp(true, 1),
    },
    methods: {
        onClose() {
            this.$emit('close');
        },
        onConfirm() {
            this.$emit('confirm');
        },
    },
};
</script>
