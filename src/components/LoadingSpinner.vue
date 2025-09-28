<script setup>
// LoadingSpinner component
</script>

<template>
  <div class="loading-spinner-container">
    <div class="loading-spinner"></div>
    <p class="loading-text">Congratulations, You Did It!</p>
    <p class="loading-text">playing now</p>
    <iframe allow="autoplay *; encrypted-media *; fullscreen *; clipboard-write" frameborder="0" height="175" style="width:100%;max-width:660px;overflow:hidden;border-radius:10px;" sandbox="allow-forms allow-popups allow-same-origin allow-scripts allow-storage-access-by-user-activation allow-top-navigation-by-user-activation" src="https://embed.music.apple.com/id/song/menghancurkan/1760139686"></iframe>
    <p class="loading-subtext">Your changes have been successfully deployed. Ade Kurniawan. v4: webhook test</p>
  </div>
</template>

<style scoped>
.loading-spinner-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #121212; /* Dark background */
  z-index: 9999;
}

.loading-spinner {
  width: 60px;
  height: 60px;
  border: 5px solid #333333;
  border-top: 5px solid #42b883; /* Vue green */
  border-radius: 50%;
  animation: spin 1.2s linear infinite;
}

.loading-text {
  margin-top: 24px;
  font-size: 24px;
  font-weight: bold;
  color: #42b883; /* Vue green */
  letter-spacing: 0.8px;
  text-shadow: 0 0 10px rgba(66, 184, 131, 0.5); /* Subtle glow */
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
