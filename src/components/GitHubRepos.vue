<template>
    <div>
        <!-- TODO: Crear componente GitHubRepos -->
         <div v-for="(item, id) in repolist" :key="id">
            <div v-if="item.language == 'HTML' || item.language == 'JavaScript'">
                {{ item.name }} - {{ item.language}}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'GitHubRepos',
    props: [
        // TODO: crear parámetro 'repolist'
        'repolist'
    ],
}
</script>

<style scoped>
</style>
