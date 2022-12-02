<template>
    <div :class="className('plansDesktop')">
        <div :class="className('shadows-d')">
            <div :class="className('plansDesktop__NameContainer')" />
            <div
                v-for="plan in plansArray"
                :key="`shadow-of-${plan}-plan`"
                :class="className('shadow-d', isActive(plans[plan].key) && 'active')"
            />
        </div>
        <div :class="className('plans-d')">
            <div :class="className('plansDesktop__NameContainer')" />
            <div
                v-for="plan in plansArray"
                :key="`information-of-${plan}-plan`"
                :class="className('plan-d', isActive(plans[plan].key) && 'active')"
            >
                <div :class="className('plan-d__info')">
                    <div :class="className('plan-d__head')">
                        <div :class="className('planIcon')">
                            <plan-icon :plan="plan" />
                        </div>
                        <div :class="className('planAttribute-d', 'name')">
                            {{ plans[plan].name }}
                        </div>
                    </div>
                    <div :class="className('plan-d__financial')">
                        <template v-if="typeof plans[plan].monthlyCost === 'number'">
                            <span :class="className('planAttribute-d', 'price')">
                                {{ plans[plan].monthlyCost | price() }}
                            </span>
                            <span :class="className('planAttribute-d', 'currency')">
                                {{ $t('support.plans.currency') }}
                            </span>
                        </template>
                        <template v-else-if="plans[plan].monthlyCost === 'growth'">
                            <div :class="className('plansDesktop__growthPlanNameContainer')" @click="openHint('growth')">
                                <span :class="className('planAttribute-d', 'currency')">
                                    {{ $t('support.plans.growthInfo.title') }}
                                </span>
                                <div
                                    :ref="'growth'"
                                    :class="className('featureHint')"
                                >
                                    <info-icon :class="className('featureHint__icon')" />
                                    <hint-box
                                        :open="showHint === 'growth'"
                                        :title=" $t('support.plans.growthInfo.title') "
                                        @close="closeHint"
                                    >
                                        {{ $t('support.plans.growthInfo.description') }}
                                    </hint-box>
                                </div>
                            </div>
                        </template>
                        <template v-else>
                            <span :class="className('planAttribute-d', 'NoNumberPrice')">
                                {{ plans[plan].monthlyCost }}
                            </span>
                        </template>
                    </div>
                </div>
                <div :class="className('plan-d__action')">
                    <div v-if="isActive(plans[plan].key)" :class="className('planAttribute-d', 'status')">
                        <ar-label variant="primary" :class="className('planAttribute-d', 'statusLabel')">
                            {{ $t('support.plans.activeLabel') }}
                        </ar-label>
                    </div>
                    <ar-button
                        v-else
                        :class="className('plan-d__button')"
                        :disabled="activatePlanLoading"
                        variant="primary"
                        @click="activatePackage({id:plan,key:plans[plan].key})"
                    >
                        <template v-if="!plans[plan].activable">
                            {{ $t('support.plans.contactUsButton') }}
                        </template>
                        <template v-else>
                            {{ $t('support.plans.activateButton') }}
                        </template>
                    </ar-button>
                </div>
            </div>
        </div>
        <div :class="className('featureSets-d')">
            <div
                v-for="featureSet in details"
                :key="`feature-set-${featureSet.id}`"
                :class="className('featureSet-d')"
            >
                <div :class="className('featureSet-d__thRow')">
                    <div :class="className('plansDesktop__NameContainer')">
                        <div :class="className('featureSetName-d')">
                            {{ featureSet.label }}
                        </div>
                    </div>
                    <div
                        v-for="plan in plansArray"
                        :key="`title-line-${plan}`"
                        :class="className('featureSet-d__thDivderContainer', isActive(plans[plan].key) && 'active')"
                    />
                </div>
                <div
                    v-for="feature in featureSet.features"
                    :key="`feature-${feature.id}`"
                    :class="className('feature-d', [showHint === feature.id && 'highlight'])"
                >
                    <div :class="className('plansDesktop__NameContainer')" @click="openHint(feature.id)">
                        <div :class="className('featureName-d')">
                            {{ feature.label }}
                        </div>
                        <div
                            v-if="Boolean(feature.description)"
                            :ref="`${feature.id}`"
                            :class="className('featureHint')"
                        >
                            <info-icon :class="className('featureHint__icon')" />
                            <hint-box
                                :open="showHint === feature.id"
                                :title="feature.label"
                                @close="closeHint"
                            >
                                {{ feature.description }}
                            </hint-box>
                        </div>
                    </div>
                    <div
                        v-for="plan in plansArray"
                        :key="`value-of-${feature.id}-in-${plan}`"
                        :class="className('feature-d__value', isActive(plans[plan].key) && 'active')"
                    >
                        <plan-feature
                            :value="feature.plans[plan].value"
                            :type="feature.plans[plan].type"
                            :tip="feature.plans[plan].tip"
                        />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
/*  eslint {indent: ['error', 4],
    semi: ['error', 'always'],
    comma-dangle: ['error', 'always-multiline']}
*/
import PlanFeature from '@arvan/support/components/PlanFeature';
import PlanIcon from '@arvan/support/components/PlanIcon';
import HintBox from '@arvan/support/components/HintBox';
import {
    booleanProp, objectProp, arrayProp, numberProp,
} from '@panel/core-modules/modules/propTypes';
import InfoIcon from './InfoIcon';
import className from '../className';

export default {
    name: 'PlansDesktop',
    components: {
        PlanFeature,
        PlanIcon,
        InfoIcon,
        HintBox,
    },
    mixins: [className],
    props: {
        activatePlanLoading: booleanProp(false, false),
        plans: objectProp(true),
        details: arrayProp(true),
        activePlan: numberProp(true),
    },
    data() {
        return {
            showHint: false,
        };
    },
    computed: {
        plansArray() {
            return Object.keys(this.plans);
        },
    },
    methods: {
        activatePackage(plan) {
            if (!this.plans[plan.id].activable) {
                this.$router.push({ path: '/support' });
                return;
            }

            this.$emit('activate-package', plan);
        },
        isActive(planKey) {
            return this.activePlan.toString() === planKey;
        },
        closeHint() {
            this.showHint = null;
        },
        openHint(featureId) {
            this.showHint = featureId;
        },
    },
};
</script>
