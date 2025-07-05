<template>
    <v-footer class="mt-8 custom-footer" padless>
        <v-row align="center" justify="center" class="py-4">
            <v-col cols="12" md="8" class="text-center">
                <span class="footer-text" style="font-size: 1.1rem;">
                    <v-icon left color="white" size="20">mdi-timer-outline</v-icon>
                    <strong>Método Cronos</strong>
                    &mdash;
                    <a href="https://linktr.ee/luiztrindade_dev" target="_blank" rel="noopener"
                        style="color: #fff; text-decoration: underline;">
                        Meu Linktree
                    </a>
                    <br>
                    <span style="opacity: 0.85;">
                        Criado por Luiz Trindade &copy; {{ new Date().getFullYear() }}
                    </span>
                </span>
            </v-col>
        </v-row>
    </v-footer>
</template>

<script setup lang="ts">
</script>

<style scoped>
.custom-footer {
    background: linear-gradient(90deg, #1976d2 0%, #42a5f5 100%);
    box-shadow: 0 2px 12px rgba(33, 150, 243, 0.2);
    color: #fff;
}

.footer-text {
    color: #fff;
    text-shadow: 0 1px 4px rgba(0, 0, 0, 0.15);
    font-weight: 500;
    letter-spacing: 0.5px;
}
</style>
