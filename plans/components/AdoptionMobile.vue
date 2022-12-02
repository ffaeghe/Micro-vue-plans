<template>
    <div :class="className('adoption')">
        <ar-row>
            <ar-col md="8">
                <span :class="className('adoptionAttribute', 'title')">
                    {{ $t('support.plans.adoptionService') }}
                </span>
                <span :class="className('adoptionAttribute', 'description')">
                    {{ $t('support.plans.adoptionDescription') }}
                </span>
            </ar-col>
            <ar-col md="4">
                <ar-button
                    :sm="true"
                    :class="className('adoptionAttribute__button')"
                    variant="primary"
                    @click="goToLink($t('support.plans.adoptionRequest.link'))"
                >
                    {{ $t('support.plans.adoptionRequest.title') }}
                </ar-button>
            </ar-col>
        </ar-row>
        <ar-row
            :class="className('adoptionPlan','border')"
        >
            <ar-col md="7">
                <span :class="className('adoptionPlan', 'title')">
                    {{ $t('support.plans.adoptionConsultant.title') }}
                </span>
            </ar-col>
            <ar-col md="5">
                <div :class="className('adoptionPlan__adoptionAmount')">
                    <span :class="className('adoptionPlan__adoptionAmount', 'price')">
                        {{ $t('support.plans.adoptionConsultant.price') }}
                    </span>
                    <span :class="className('adoptionPlan__adoptionAmount', 'currency')">
                        {{ $t('support.plans.adoptionConsultant.currency') }}

                    </span>
                    <span :class="className('adoptionPlan__adoptionAmount', 'per')">
                        /{{ $t('support.plans.adoptionConsultant.per') }}
                    </span>
                </div>
            </ar-col>
            <ar-col
                md="12"
                :class="className('hdivier','withUpDownMargin')"
            />
            <ar-col md="7">
                <span :class="className('adoptionPlan', 'title')">
                    {{ $t('support.plans.adoptionImplement.title') }}
                </span>
            </ar-col>
            <ar-col md="5">
                <div :class="className('adoptionPlan__adoptionAmount')">
                    <span :class="className('adoptionPlan__adoptionAmount', 'price')">
                        {{ $t('support.plans.adoptionImplement.price') }}
                    </span>
                    <span :class="className('adoptionPlan__adoptionAmount', 'currency')">
                        {{ $t('support.plans.adoptionImplement.currency') }}

                    </span>
                    <span :class="className('adoptionPlan__adoptionAmount', 'per')">
                        /{{ $t('support.plans.adoptionImplement.per') }}
                    </span>
                </div>
            </ar-col>
            <ar-row />
        </ar-row>
    </div>
</template>
<script>

/*  eslint {indent: ['error', 4],
    semi: ['error', 'always'],
    comma-dangle: ['error', 'always-multiline']}
*/
import helper from '@arvan/support/mixins/helper';
import className from '../className';

export default {
    name: 'AdoptionMobile',
    mixins: [className, helper],
    methods: {
        onClick() {
            if (this.contactUs) {
                this.$router.push({ path: '/support' });
            }

            this.$emit('activate', this.id);
        },
    },
};
</script>
