<template>
    <div :class="className('plan-m', active && 'active')">
        <div :class="className('planIcon')">
            <plan-icon :plan="id" />
        </div>
        <div :class="className('planAttribute-m', ['name', active && 'activeName'])">
            {{ name }}
        </div>
        <div :class="className('planAttribute-m', 'group')">
            <div :class="className('planAttribute-m', 'price')">
                <template v-if="typeof price === 'number'">
                    <span>
                        {{ price | price() }}
                    </span>
                    <span>
                        {{ currency }}
                    </span>
                </template>
                <template v-else-if="price === 'growth'">
                    <span :class="className('planAttribute-d', 'growth')">
                        {{ $t('support.plans.growthInfo.title') }}
                    </span>
                </template>
            </div>
            <div v-if="typeof price === 'number'" :class="className('planAttribute-m', 'paymentPeriod')">
                {{ paymentPeriod }}
            </div>
        </div>
        <div v-if="active" :class="className('planAttribute-m', 'status')">
            <ar-label variant="primary" :class="className('planAttribute-m', 'statusLabel')">
                {{ $t('support.plans.activeLabel') }}
            </ar-label>
        </div>
        <div v-else :class="className('plan-m__action')">
            <ar-button
                :class="className('plan-m__button')"
                :disabled="loading"
                variant="primary"
                @click="onClick"
            >
                <template v-if="contactUs">
                    {{ $t('support.plans.contactUsButton') }}
                </template>
                <template v-else>
                    {{ $t('support.plans.activateButton') }}
                </template>
            </ar-button>
        </div>
        <div :class="className('divider', 'fullWidth')" />
        <div
            v-for="(featureSet, index) in featureSets"
            :key="`plan-${id}-feature-set-${featureSet.id}`"
            :class="className('featureSet-m')"
        >
            <div :class="className('featureTitle-m','parent')">
                {{ featureSet.label }}
            </div>
            <div
                v-for="feature in featureSet.features"
                :key="`plan-${id}-feature-${feature.id}`"
                :class="className('feature-m')"
            >
                <div :class="className('featureTitle-m', [feature.type=== 'parent' && 'parent'])">
                    {{ feature.label }}
                </div>
                <plan-feature
                    :value="feature.plans[id].value"
                    :type="feature.plans[id].type"
                />
            </div>
            <div v-if="index < featureSets.length - 1" :class="className('divider', 'fullWidth')" />
        </div>
    </div>
</template>
<script>
import PlanFeature from '@arvan/support/components/PlanFeature';
import PlanIcon from '@arvan/support/components/PlanIcon';

import {
    arrayProp, booleanProp, stringProp, numberProp, unionProp,
} from '@panel/core-modules/modules/propTypes';
/*  eslint {indent: ['error', 4],
    semi: ['error', 'always'],
    comma-dangle: ['error', 'always-multiline']}
*/
import className from '../className';

export default {
    name: 'PlanMobile',
    components: {
        PlanFeature,
        PlanIcon,
    },
    mixins: [className],
    props: {
        name: stringProp(true, ''),
        price: unionProp([String, Number]),
        active: booleanProp(true, false),
        featureSets: arrayProp(true, []),
        loading: booleanProp(false, false),
        id: numberProp(true, -1),
        contactUs: booleanProp(false, false),
        currency: stringProp(false, ''),
        planKey: stringProp(true, ''),
        paymentPeriod: stringProp(false, ''),
    },
    methods: {
        onClick() {
            if (this.contactUs) {
                this.$router.push({ path: '/support' });
            }

            this.$emit('activate', { id: this.id, key: this.planKey });
        },
    },
};
</script>
