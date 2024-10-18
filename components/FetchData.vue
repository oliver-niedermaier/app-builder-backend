<!-- ========================================================================== -->
<!--                                  TEMPLATE                                  -->
<!-- ========================================================================== -->
<template>
    <main>
        <h1>
            {{ app.title }} <code>{{ app.id }}</code>
        </h1>

        <!-- plain html -->
        <div class="_dev-plain-html">
            <ul>
                <li v-for="page in app.structure.items" :key="page.id">
                    <code> 🧬 {{ Object.keys(page)[0] }} </code>
                    <code> 🆔 {{ page.id }} </code>
                    <code> 🏷️ {{ page.title }} </code>
                    <code> 🔗 {{ page.route }} </code>
                </li>
            </ul>
        </div>

        <!-- prime vue component -->
        <Fieldset legend="app-structure">
            <TreeTable
                :value="treeNodes"
                :data-key="'key'"
                resizable-columns
                row-hover
                size="small"
                show-gridlines
            >
                <Column field="title" header="🏷️ Title" expander />
                <Column field="type" header="🧬 Type">
                    <template #body="row">
                        <code>
                            {{ row.node.data.id }}
                        </code>
                    </template>
                </Column>
                <Column field="id" header="🆔 ID">
                    <template #body="row">
                        <code>
                            {{ row.node.data.id }}
                        </code>
                    </template>
                </Column>
                <Column field="route" header="🔗 Route">
                    <template v-if="row.node.data.route" #body="row">
                        <div style="display: grid; justify-items: start">
                            <code>
                                {{ row.node.data.route }}
                            </code>

                            <Button as="a" :href="row.node.data.route" link>
                                ↬ href
                            </Button>

                            <Button link>
                                <NuxtLink :to="row.node.data.route">
                                    ⇢ nuxt-link
                                </NuxtLink>
                            </Button>
                        </div>
                    </template>
                </Column>
            </TreeTable>
        </Fieldset>

        <!-- raw page data -->
        <Panel
            v-for="page in app.structure.items"
            :key="page.id"
            :header="page.title"
            toggleable
            collapsed
        >
            <pre>{{ JSON.stringify(page, null, 4) }}</pre>
        </Panel>
    </main>
</template>

<!-- ========================================================================== -->
<!--                                   SCRIPT                                   -->
<!-- ========================================================================== -->
<script setup>
// import app data yaml file
const { app } = await queryContent("data/my-awesome-app").findOne();

// function to transform items into TreeNode format
// only needed to fit to primevue TreeTable component
function generateTreeNodes(items) {
    return items.map((item) => ({
        key: item.id,
        data: {
            id: item.id,
            title: item.title,
            type: item.component || Object.keys(item)[0],
            route: item.route,
        },
        children: item.items ? generateTreeNodes(item.items) : null,
    }));
}

const treeNodes = ref(generateTreeNodes(app.structure.items));
</script>

<!-- ========================================================================== -->
<!--                                   STYLES                                   -->
<!-- ========================================================================== -->
<style scoped>
main {
    padding: 1em;
}
main > * {
    margin-bottom: 1em;
}

h1 {
    font-size: 1.5em;
    font-weight: bold;
}

code {
    font-size: 0.9em;
    opacity: 0.7;
}

._dev-plain-html {
    display: none;
}
</style>
