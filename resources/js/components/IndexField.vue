<template>
    <div>
        <button :class="addClass" @click="changeCreateBtn">{{field.value.totalRelationship}} {{field.value.title}}
        </button>
        <modal v-if="showModal" @modal-close="handleClose">
            <form
                    slot-scope="props"
                    class="bg-white rounded-lg shadow-lg overflow-hidden col-lg-12"
                    style="width: 90%"
            >
                <slot :uppercaseMode="uppercaseMode" :mode="mode">
                    <div class="p-8">


                        <div class="card">
                            <button class="btn btn-default btn-primary" @click.prevent="showCreateForm = true">Create
                            </button>
                            <create
                                    v-if="showCreateForm"
                                    :resource-name="field.resourceName"
                                    :via-resource="resourceName"
                                    :via-resource-id="field.value.resourceId"
                                    :via-relationship="field.hasManyRelationship"
                                    :relationship-type="'hasMany'"
                                    @close="onCreated"
                            ></create>
                        </div>
                        <br/>
                        <Index
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

        <!--
          <modal v-if="showModalCreate" @modal-close="handleCreateClose">
              <form
                      slot-scope="props"
                      class="bg-white rounded-lg shadow-lg overflow-hidden col-lg-12"
                      style="width: 90%"
              >
                        <slot :uppercaseMode="uppercaseMode" :mode="mode">
                            <div class="p-8">
                                <create
                                        :resource-name="field.resourceName"
                                        :via-resource="resourceName"
                                        :via-resource-id="field.value.resourceId"
                                        :via-relationship="field.hasManyRelationship"
                                        :relationship-type="'hasMany'"
                                ></create>
                            </div>
                        </slot>

                        <div class="bg-30 px-6 py-3 flex">
                            <div class="ml-auto">
                                <button
                                        type="button"
                                        data-testid="cancel-button"
                                        dusk="cancel-delete-button"
                                        @click.prevent="handleCreateClose"
                                        class="btn text-80 font-normal h-9 px-3 mr-3 btn-link"
                                >
                                    {{ __('Cancel') }}
                                </button>

                            </div>
                        </div>
                    </form>
            </modal>
        -->


    </div>
</template>

<script>
    // import Create from "../../../../../../nova/resources/js/views/Create";
    import Create from "./Create";
    import Index from "./Index";
    //  import ResourceCreate from '@nova/views/Create'


    export default {
        components: {Index, Create},
        data() {
            return {
                'showModal': false,
                'showModalCreate': false,
                'addClass': '',
                'showCreateForm': false,

            }
        },
        props: ['resourceName', 'resourceId', 'resource', 'field'],

        methods: {
            tabClicked(selectedTab) {
                console.log('Current tab re-clicked:' + selectedTab.tab.name);
            },
            tabChanged(selectedTab) {
                console.log('Tab changed to:' + selectedTab.tab.name);
            },
            onCreated() {
                this.setCount()
                this.addClass = 'btn-primary '
                this.showModal = false;
            },
            handleClose() {
                this.showModal = false;
            },

            setCount() {
                Nova.request()
                    .get('/nova-api/' + this.resourceName + '/count', {
                        params: {
                            viaResource: this.resourceName,
                            viaResourceId: this.field.value.resourceId,
                            viaRelationship: this.field.hasManyRelationship,
                            relationshipType: 'hasMany'
                        },
                    })
                    .then(response => {
                        this.field.value.totalRelationship = response.data.count
                    })
            },

            changeCreateBtn() {
                this.showModal = true;
            },
            actionExecuted() {
                this.$emit('actionExecuted')
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
