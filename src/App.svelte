<script lang="ts">
  import ImageSelector from './components/ImageSelector.svelte';
  import CameraCapture from './components/CameraCapture.svelte';
  import IDForm from './components/IDForm.svelte';
  import type { IDData } from './types';
  import { processImage } from './utils/ocr';

  let idData: IDData | null = null;
  let error: string | null = null;

  async function handleImageSelected(event: CustomEvent<File>) {
    try {
      error = null;
      idData = await processImage(event.detail);
    } catch (err) {
      error = err instanceof Error ? err.message : 'An unknown error occurred';
      idData = null;
    }
  }

  async function handleImageCaptured(event: CustomEvent<Blob>) {
    try {
      error = null;
      idData = await processImage(event.detail);
    } catch (err) {
      error = err instanceof Error ? err.message : 'An unknown error occurred';
      idData = null;
    }
  }
</script>

<main>
  <h1>ID Card Scanner</h1>
  
  <div class="actions">
    <ImageSelector on:imageSelected={handleImageSelected} />
    <CameraCapture on:imageCaptured={handleImageCaptured} />
  </div>

  {#if error}
    <p class="error">{error}</p>
  {/if}

  {#if idData}
    <IDForm bind:idData />
  {/if}
</main>

<style>
  main {
    padding: 20px;
    max-width: 600px;
    margin: 0 auto;
  }

  .actions {
    display: flex;
    justify-content: space-around;
    margin-bottom: 20px;
  }

  .error {
    color: red;
    margin-top: 10px;
  }
</style>