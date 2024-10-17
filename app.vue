<template>
    <div>
        <FetchData />

        <NuxtRouteAnnouncer />
        <NuxtWelcome :title="appTitle" :app-name="appName" />
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
