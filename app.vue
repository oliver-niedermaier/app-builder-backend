<template>
    <div>
        <FetchData />

        <NuxtWelcome
            :title="appTitle"
            :app-name="appName"
            style="display: none"
        />
    </div>
</template>

<script setup lang="ts">
const appName = "app-builder-nuxt";
const appTitle = "App Builder · Nuxt";

useHead({
    title: appTitle,
    link: [
        {
            rel: "icon",
            type: "image/svg+xml",
            href: "/mage.svg",
        },
    ],
});
</script>
