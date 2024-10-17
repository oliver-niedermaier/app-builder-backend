<template>
    <div style="padding: 1em">
        <h1>
            {{ app.title }} <code>{{ app.id }}</code>
        </h1>

        <!-- plain html -->
        <div style="display: none">
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
        <TreeTable
            :value="treeNodes"
            resizable-columns
            row-hover
            size="small"
            show-gridlines
        >
            <Column field="title" header="🏷️ Title" expander />
            <Column field="type" header="🧬 Type" />
            <Column field="id" header="🆔 ID" />
            <Column field="route" header="🔗 Route" />
        </TreeTable>
    </div>
</template>

<script setup>
// import app data yaml file
const { app } = await queryContent("data/app.data").findOne();

// function to transform items into TreeNode format
// only needed to fit to primevue TreeTable component
function generateTreeNodes(items) {
    return items.map((item) => ({
        data: {
            id: item.id,
            title: item.title,
            type: Object.keys(item)[0],
            route: item.route,
        },
        children: item.items ? generateTreeNodes(item.items) : null,
    }));
}

const treeNodes = ref(generateTreeNodes(app.structure.items));
</script>
