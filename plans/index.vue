<template>
    <ar-container v-if="hasFetchedUserPlan">
        <ar-content>
            <template>
                <div :class="className('header')">
                    <div :class="className('header__title')">
                        {{ $t('support.plans.title') }}
                        <div :class="className('header__help')">
                            <a :class="className('help')" href="#" @click="goToLink($t('support.plans.moreInfo.link'))">
                                <ar-icon name="alert" :class="className('help__icon')" />
                            </a>
                        </div>
                    </div>
                </div>
                <div :class="className('description')">
                    {{ $t('support.plans.description') }}
                </div>
                <div :class="className('body')">
                    <PlansMobile
                        v-if="isSmallScreen"
                        :plans="plans"
                        :details="plansDetails"
                        :active-plan="userPlan.activePlan"
                        :activate-plan-loading="activatePlanLoading"
                        @activate-package="openConfirmModal"
                    />
                    <plans-desktop
                        v-else
                        :plans="plans"
                        :details="plansDetails"
                        :active-plan="userPlan.activePlan"
                        :activate-plan-loading="activatePlanLoading"
                        @activate-package="openConfirmModal"
                    />
                    <adoption-mobile v-if="isSmallScreen" />
                    <adoption v-else />
                </div>
                <div :class="className('footer')">
                    <a :class="className('moreInfo')" href="#" @click="goToLink($t('support.plans.moreInfo.link'))">
                        <external-link-icon :class="className('moreInfo__icon')" />
                        <span :class="className('moreInfo__text')">
                            {{ $t('support.plans.moreInfo.text') }}
                        </span>
                    </a>
                </div>
            </template>
        </ar-content>
        <confirm-modal
            :show="showConfirmModal"
            :plan="selectedPlan"
            :loading="activatePlanLoading"
            @close="closeConfirmModal"
            @confirm="activePackage"
        />
    </ar-container>
    <skeleton v-else />
</template>
<script>
/*  eslint {indent: ['error', 4],
    semi: ['error', 'always'],
    comma-dangle: ['error', 'always-multiline']}
*/
import { mapGetters } from 'vuex';
import Adoption from '~/products/support/pages/plans/components/Adoption';
import AdoptionMobile from '~/products/support/pages/plans/components/AdoptionMobile';
import { selectPlan } from '~/products/support/api/plan';
import Skeleton from '~/components/DdosSkeleton';
import helper from '~/products/support/mixins/helper';
import className from './className';
import PlansDesktop from './components/PlansDesktop';
import PlansMobile from './components/PlansMobile';
import ConfirmModal from './components/ConfirmModal';
import ExternalLinkIcon from './components/ExternalLinkIcon';

export default {
    name: 'PlansPage',
    components: {
        Skeleton,
        Adoption,
        AdoptionMobile,
        ConfirmModal,
        PlansDesktop,
        PlansMobile,
        ExternalLinkIcon,
    },
    mixins: [className, helper],
    data() {
        return {
            activatePlanLoading: false,
            showConfirmModal: false,
            selectedPlan: -1,
            selectedPlanKey: -1,
        };
    },
    computed: {
        ...mapGetters('support', ['userPlan', 'hasFetchedUserPlan']),
        ...mapGetters('layout', ['breakpoint']),
        isSmallScreen() {
            return this.breakpoint !== 'xxl' && this.breakpoint !== 'xl';
        },
        plans() {
            return this.$t('support.newPlans');
        },
        plansDetails() {
            return [
                this.$t('support.newPlanDetails'),
            ];
        },
    },
    methods: {
        openConfirmModal(plan) {
            if (plan.id === 4) {
                return;
            }
            this.selectedPlan = plan.id;
            this.selectedPlanKey = plan.key;
            this.showConfirmModal = true;
        },
        activePackage() {
            if (!this.selectedPlan) {
                return;
            }
            this.activatePlanLoading = true;
            selectPlan({ plan: this.selectedPlanKey })
                .finally(() => {
                    this.closeConfirmModal();
                    this.activatePlanLoading = false;
                });
        },
        closeConfirmModal() {
            this.showConfirmModal = false;
            this.selectedPlan = -1;
        },
        toDashboard() {
            this.$router.push({
                name: 'cdn.dashboard',
                params: {
                    domain: this.activeDomain.domain,
                },
            });
        },
    },
};
</script>
