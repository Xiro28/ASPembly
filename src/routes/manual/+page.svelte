<script lang="ts">
  import { asset } from '$app/paths';

  const pdfUrl = asset('/1.pdf');
</script>

<svelte:head>
  <title>Manual</title>
</svelte:head>

<div class="manual-page">
  <div class="manual-header">
    <h1>Manuale</h1>

    <a href={pdfUrl} target="_blank" rel="noopener noreferrer">
      Open PDF
    </a>
  </div>

  <iframe
    src={pdfUrl}
    title="Manuale PDF"
    class="pdf-viewer"
  ></iframe>
</div>

<style>
  .manual-page {
    min-height: 100dvh;
    display: flex;
    flex-direction: column;
    padding: 1rem;
    box-sizing: border-box;
  }

  .manual-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 1rem;
    margin-bottom: 1rem;
  }

  .manual-header h1 {
    margin: 0;
    font-size: clamp(1.3rem, 4vw, 2rem);
  }

  .manual-header a {
    white-space: nowrap;
  }

  .pdf-viewer {
    flex: 1;
    width: 100%;
    min-height: 70dvh;
    border: 1px solid #ddd;
    border-radius: 8px;
  }

  @media (max-width: 600px) {
    .manual-page {
      padding: 0.75rem;
    }

    .manual-header {
      flex-direction: column;
      align-items: flex-start;
    }

    .pdf-viewer {
      min-height: 75dvh;
    }
  }
</style>