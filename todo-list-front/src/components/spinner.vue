<template>
    <div class="spinner"></div>
</template>

<style scoped>
.spinner {
    border: 4px solid rgb(43, 255, 0);
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border-left-color: rgb(255, 0, 221);
    animation: spin 1s ease infinite;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}
</style>