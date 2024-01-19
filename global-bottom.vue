<template>
    <footer class="absolute bottom-0 left-3 right-0 p-2 z-50">
        <LightOrDark>
            <template #dark>
                <img src="/zioLogo.png" class="h-10" />
            </template>
            <template #light>
                <img src="/zioLogo.png" class="h-10" />
            </template>
        </LightOrDark>
    </footer>

    <footer class="absolute bottom-0 right-12 p-2 z-50">
        <SlideCurrentNo /> /
        <SlidesTotal />
    </footer>
</template>