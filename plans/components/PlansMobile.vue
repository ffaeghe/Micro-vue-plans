<template>
    <div :class="className('plansMobile')">
        <div :class="className('plans-m')">
            <plan-mobile
                v-for="plan in plansArray"
                :id="plan"
                :key="`support-plan-${plan.id}`"
                :plan-key="plans[plan].key"
                :name="plans[plan].name"
                :active="isActive(plans[plan].key)"
                :price="plans[plan].monthlyCost"
                :currency="$t('support.plans.currency')"
                :payment-period="$t('support.plans.paymentPeriod')"
                :contact-us="!plans[plan].activable"
                :feature-sets="details"
                @activate="activatePackage"
            />
        </div>
    </div>
</template>
<script>
/*  eslint {indent: ['error', 4],
    semi: ['error', 'always'],
    comma-dangle: ['error', 'always-multiline']}
*/
import {
    booleanProp, objectProp, arrayProp, numberProp,
} from '@panel/core-modules/modules/propTypes';
import className from '../className';
import PlanMobile from './PlanMobile';

export default {
    name: 'PlansMobile',
    components: {
        PlanMobile,
    },
    mixins: [className],
    props: {
        activatePlanLoading: booleanProp(false, false),
        plans: objectProp(true),
        details: arrayProp(true),
        activePlan: numberProp(true),
    },
    computed: {
        plansArray() {
            return Object.keys(this.plans);
        },
    },
    methods: {
        activatePackage(plan) {
            this.$emit('activate-package', plan);
        },
        isActive(planKey) {
            return this.activePlan.toString() === planKey;
        },
    },
};
</script>
