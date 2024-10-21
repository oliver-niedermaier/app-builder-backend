<!-- ========================================================================== -->
<!--                             COMPONENT FACTORY                              -->
<!-- ========================================================================== -->
<template>
    <main>
        <div v-for="item in items" :key="item.id" class="app-page">
            <!-- <code>🧬 {{ item.component }}</code> -->

            <InputText v-if="!item.component" :v-model="item.id + '_val'" />

            <!-- INPUT -->
            <ix-input
                v-else-if="
                    item.type === 'text' ||
                    item.type === 'email' ||
                    item.type === 'password'
                "
            >
                <label v-if="item.label" :for="item.id">{{ item.label }}</label>

                <Password
                    v-if="item.type === 'password'"
                    :v-model="item.id + '_val'"
                    :feedback="false"
                />
                <InputText
                    v-else
                    id="item.id"
                    :v-model="item.id + '_val'"
                    :placeholder="item.placeholder"
                />

                <small v-if="item.helpText" :id="item.id + '--help'">
                    {{ item.helpText }}
                </small>
            </ix-input>

            <!-- GROUP -->
            <Panel
                v-else-if="item.component === 'ab-group'"
                :header="!item.settings.noHeader ? item.title : null"
                :toggleable="item.settings.collapsible"
            >
                <ComponentFactory :items="item.items" />
            </Panel>
        </div>
    </main>
</template>

<!-- ========================================================================== -->
<!--                                   SCRIPT                                   -->
<!-- ========================================================================== -->
<script>
export default {
    name: "ComponentFactory",
    props: {
        items: {
            type: Array,
            required: true,
        },
    },
    data() {
        return {
            value: "",
        };
    },
};
</script>

<!-- ========================================================================== -->
<!--                                   STYLES                                   -->
<!-- ========================================================================== -->
<style scoped>
main {
    padding: 0.5em;
}
main > * {
    margin-bottom: 1em;
}

ix-input {
    display: flex;
    flex-direction: column;
    width: 200px;
    gap: 0.4em;
}
</style>

<style>
.p-panel-header {
    background-color: var(--p-treetable-row-hover-background);
    border-top-left-radius: var(--p-panel-border-radius);
    border-top-right-radius: var(--p-panel-border-radius);
}
</style>
