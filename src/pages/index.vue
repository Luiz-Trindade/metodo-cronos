<template>
    <v-container>
        <v-row class="mb-4" align="center" justify="space-between">
            <v-col cols="auto">
                <v-btn color="primary" @click="addTracker">
                    <v-icon left>mdi-plus</v-icon>
                    Adicionar Rastreador
                </v-btn>
            </v-col>
            <v-col cols="auto">
                <v-switch v-model="isDark" :label="isDark ? '🌙 Modo Escuro' : '☀️ Modo Claro'" inset />
            </v-col>
        </v-row>

        <div v-if="trackers.length > 0">
            <Cronos v-for="(item, index) in trackers" :key="index" v-model:title="item.title" :total-days="item.totalDays"
                :days="item.days" @update:days="val => updateDays(index, val)" active-color="green" inactive-color="red"
                @remove="removeTracker(index)" />
        </div>

        <div v-else class="display-flex text-center justify-center align-center">
            <p class="text-center text-grey">
                Nenhum rastreador adicionado ainda. Clique em "Adicionar Rastreador" para começar.
            </p>
        </div>
    </v-container>
</template>

<script setup lang="ts">
import { watch, onMounted, ref } from 'vue';

type Tracker = {
    title: string;
    totalDays: number;
    days: number[];
};

const trackers = ref<Tracker[]>([]);

onMounted(() => {
    const saved = localStorage.getItem('cronos_trackers');
    if (saved) {
        try {
            const parsed = JSON.parse(saved);
            if (Array.isArray(parsed)) {
                parsed.forEach((item) => {
                    if (
                        typeof item.title === 'string' &&
                        typeof item.totalDays === 'number' &&
                        Array.isArray(item.days)
                    ) {
                        trackers.value.push(item);
                    }
                });
            }
        } catch (e) {
            console.error('Erro ao carregar rastreadores salvos:', e);
        }
    }
});

watch(
    trackers,
    (val) => {
        localStorage.setItem('cronos_trackers', JSON.stringify(val));
    },
    { deep: true }
);

function addTracker() {
    trackers.value.push({
        title: 'Novo Rastreador',
        totalDays: 30,
        days: Array(30).fill(0),
    });
}

function removeTracker(index: number) {
    trackers.value.splice(index, 1);
}

function updateDays(index: number, days: number[]) {
    trackers.value[index].days = days;
}

import { useTheme } from 'vuetify'

const isDark = ref(false)
const theme = useTheme()

onMounted(() => {
    const storedTheme = localStorage.getItem('appTheme')
    isDark.value = storedTheme === 'dark'
    theme.global.name.value = isDark.value ? "dark" : "light";
})

watch(isDark, (val) => {
    const newTheme = val ? "dark" : "light";
    localStorage.setItem('appTheme', newTheme);
    theme.global.name.value = newTheme;
})
</script>
