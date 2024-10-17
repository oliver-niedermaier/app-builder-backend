<template>
    <div>
        <h1>{{ app.title }}</h1>
        <strong>Pages:</strong>
        <ul>
            <li v-for="page in app.structure.items" :key="page.id">
                <code> 🧬 {{ Object.keys(page)[0] }} </code>
                <code> 🆔 {{ page.id }} </code>
                <code> 🏷️ {{ page.title }} </code>
                <code> 🔗 {{ page.route }} </code>
            </li>
        </ul>
    </div>
</template>

<script setup>
const { app } = await queryContent("data/app.data").findOne();
</script>
