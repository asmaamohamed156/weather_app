<template>
    <div>
        <Suspense>
            <AsyncCityView/>
            <template #fallback>
                <CityCardSkeleton/>
            </template>
        </Suspense>
    </div>
</template>

<script setup>
import AsyncCityView from '../components/AsyncCityView.vue';
import CityCardSkeleton from '../components/CityCardSkeleton.vue';
 

</script>
 