<script setup lang="ts">
import { computed, defineProps, defineEmits, ref, watch } from 'vue';

const props = defineProps<{
    title: string;
    totalDays?: number;
    activeColor?: string;
    inactiveColor?: string;
    days: number[];
}>();

const emit = defineEmits<{
    (e: 'update:title', value: string): void;
    (e: 'update:days', value: number[]): void;
    (e: 'remove'): void;
}>();

const editableTitle = computed({
    get: () => props.title,
    set: (val) => emit('update:title', val),
});

// days local para manipulação
const localDays = ref<number[]>([]);

// Inicializa localDays quando prop mudar
watch(
    () => props.days,
    (newVal) => {
        localDays.value = [...newVal];
    },
    { immediate: true }
);

function toggleDay(index: number) {
    localDays.value[index] = (localDays.value[index] + 1) % 3;
    emit('update:days', [...localDays.value]);
}

function buttonColor(state: number) {
    if (state === 1) return props.activeColor ?? 'green';
    if (state === 2) return props.inactiveColor ?? 'red';
    return '';
}

const progress = computed(() => {
    const greens = localDays.value.filter((d) => d === 1).length;
    return (greens / (localDays.value.length || 1)) * 100;
});
</script>

<template>
    <v-card class="pa-2 mb-4 elevation-4" style="border-left: 5px solid aqua;">
        <v-row class="mb-2">
            <v-col cols="8">
                <v-text-field v-model="editableTitle" label="Título" />
            </v-col>

            <v-col cols="2" class="d-flex align-center justify-center">
                <v-progress-circular :model-value="progress" size="56" width="6" :color="activeColor">
                    {{ Math.round(progress) }}%
                </v-progress-circular>
            </v-col>

            <v-col cols="2" class="d-flex justify-center align-center">
                <v-btn icon @click="emit('remove')" color="red" variant="text">
                    <v-icon>mdi-delete</v-icon>
                </v-btn>
            </v-col>
        </v-row>

        <v-expansion-panels>
            <v-expansion-panel>
                <template #title>
                    <v-icon class="mr-2">mdi-calendar</v-icon>
                    Acompanhar Progresso
                </template>

                <v-expansion-panel-text>
                    <v-row dense class="mb-2">
                        <v-col v-for="(state, index) in localDays" :key="index" cols="12" sm="6" md="4" lg="3">
                            <v-btn class="elevation-4 rounded" variant="flat" block :color="buttonColor(state)"
                                @click="toggleDay(index)">
                                {{ index + 1 }}
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-expansion-panel-text>
            </v-expansion-panel>
        </v-expansion-panels>
    </v-card>
</template>
