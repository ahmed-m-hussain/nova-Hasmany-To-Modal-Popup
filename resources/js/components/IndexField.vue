<template>
    <span>
    <button @click="showModal = true">{{field.value.totalRelationship}} Comments</button>
    <modal v-if="showModal" @modal-close="handleClose">
  <form
          @submit.prevent="handleConfirm"
          slot-scope="props"
          class="bg-white rounded-lg shadow-lg overflow-hidden"
          style="width: 920px"
  >
            <slot :uppercaseMode="uppercaseMode" :mode="mode">
                <div class="p-8">

    <resource-index
            :field="field"
            :resource-name="field.resourceName"
            :via-resource="resourceName"
            :via-resource-id="field.value.resourceId"
            :via-relationship="field.hasManyRelationship"
            :relationship-type="'hasMany'"
            @actionExecuted="actionExecuted"
            :load-cards="false"
            :initialPerPage="field.perPage || 5"
    />
                </div>
            </slot>

            <div class="bg-30 px-6 py-3 flex">
                <div class="ml-auto">
                    <button
                            type="button"
                            data-testid="cancel-button"
                            dusk="cancel-delete-button"
                            @click.prevent="handleClose"
                            class="btn text-80 font-normal h-9 px-3 mr-3 btn-link"
                    >
                        {{ __('Cancel') }}
                    </button>

                </div>
            </div>
        </form>
    </modal>
        </span>
</template>

<script>
    export default {
        data() {
            return {
                'showModal': false,
            }
        },
        props: ['resourceName', 'resourceId', 'resource', 'field'],

        /*props: {
            mode: {
                type: String,
                default: 'delete',
                validator: function (value) {
                    return ['force delete', 'delete', 'detach'].indexOf(value) !== -1
                },
            },
        },*/

        methods: {
            handleClose() {
                this.showModal = false;
            },
            actionExecuted() {
                this.$emit('actionExecuted')
            },
            handleConfirm() {
                this.$emit('confirm')
            },
        },

        /**
         * Mount the component.
         */
        mounted() {

        },

        computed: {
            uppercaseMode() {
                return _.startCase(this.mode)
            },
        },
    }
</script>
