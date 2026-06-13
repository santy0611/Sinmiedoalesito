<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<meta name="color-scheme" content="dark" />
<title>Continuar</title>

<style>
  /* ====================================================================
     1) VARIABLES GLOBALES
     Cambia estos valores para personalizar colores, tipografía y la
     velocidad de las animaciones sin tocar el resto del CSS.
  ==================================================================== */
  :root {
    /* --- Paleta: oscuro elegante --- */
    --color-fondo: #0c0c0d;
    --color-texto: #f5f5f4;
    --color-texto-secundario: #8a8a8d;

    --color-boton-fondo: #f5f5f4;
    --color-boton-fondo-hover: #ffffff;
    --color-boton-texto: #0c0c0d;

    /* Alternativa "modo claro": descomenta estos valores y ajusta
       el resplandor de la sección "LAYOUT PRINCIPAL" (o elimínalo)
       --color-fondo: #ffffff;
       --color-texto: #0c0c0d;
       --color-texto-secundario: #6b6b6f;
       --color-boton-fondo: #0c0c0d;
       --color-boton-fondo-hover: #262626;
       --color-boton-texto: #ffffff;
    */

    /* --- Tipografía: fuentes del sistema, sin librerías externas --- */
    --fuente-principal: -apple-system, BlinkMacSystemFont, "Segoe UI",
      "Inter", Roboto, Helvetica, Arial, sans-serif;

    /* --- Animaciones --- */
    --easing-suave: cubic-bezier(0.16, 1, 0.3, 1);
    --duracion-salida: 0.4s;  /* desvanecido del botón */
    --duracion-entrada: 0.7s; /* aparición de la imagen */
  }

  /* ====================================================================
     2) RESET BÁSICO
  ==================================================================== */
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  html,
  body {
    height: 100%;
  }

  /* ====================================================================
     3) LAYOUT PRINCIPAL
     Centrado vertical y horizontal con flexbox.
  ==================================================================== */
  body {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: clamp(2rem, 6vw, 3.5rem);
    padding: 2rem;
    text-align: center;

    font-family: var(--fuente-principal);
    color: var(--color-texto);
    background-color: var(--color-fondo);

    /* Resplandor sutil y centrado: único detalle decorativo de la página */
    background-image: radial-gradient(
      ellipse at 50% 45%,
      rgba(255, 255, 255, 0.05),
      transparent 60%
    );

    -webkit-font-smoothing: antialiased;
  }

  /* ====================================================================
     4) BOTÓN "CONTINUAR"
  ==================================================================== */
  .btn-continuar {
    font-family: inherit;
    font-size: clamp(0.95rem, 0.85rem + 0.4vw, 1.05rem);
    font-weight: 600;
    letter-spacing: 0.02em;

    padding: 1.1rem 2.75rem;
    border: none;
    border-radius: 999px; /* forma de pastilla */
    cursor: pointer;

    color: var(--color-boton-texto);
    background-color: var(--color-boton-fondo);

    transition:
      opacity var(--duracion-salida) var(--easing-suave),
      transform var(--duracion-salida) var(--easing-suave),
      background-color var(--duracion-salida) var(--easing-suave),
      box-shadow var(--duracion-salida) var(--easing-suave);
  }

  .btn-continuar:hover {
    background-color: var(--color-boton-fondo-hover);
    transform: translateY(-2px);
    box-shadow: 0 12px 30px rgba(255, 255, 255, 0.08);
  }

  .btn-continuar:active {
    transform: translateY(0) scale(0.97);
  }

  .btn-continuar:focus-visible {
    outline: 2px solid rgba(255, 255, 255, 0.45);
    outline-offset: 4px;
  }

  /* Estado al hacer clic: el botón se desvanece antes de retirarse */
  .btn-continuar.is-hidden {
    opacity: 0;
    transform: scale(0.97);
    pointer-events: none;
  }

  /* ====================================================================
     5) CONTENEDOR DE LA IMAGEN
     Empieza oculto (display: none) para no dejar espacio vacío antes
     del clic. JS lo activa y añade ".is-visible" para la transición.
  ==================================================================== */
  .imagen-wrapper {
    display: none;
    width: 100%;
    max-width: min(90vw, 560px);

    opacity: 0;
    transform: translateY(24px) scale(0.98);
    transition:
      opacity var(--duracion-entrada) var(--easing-suave),
      transform var(--duracion-entrada) var(--easing-suave);
  }

  .imagen-wrapper.is-visible {
    opacity: 1;
    transform: translateY(0) scale(1);
  }

  .imagen-wrapper img {
    display: block;
    width: auto;
    height: auto;
    max-width: 100%;
    max-height: 65vh;
    margin: 0 auto;
    border-radius: 20px;
    box-shadow:
      0 24px 60px rgba(0, 0, 0, 0.45),
      0 0 0 1px rgba(255, 255, 255, 0.06);
  }

  /* Mensaje de respaldo si la imagen no llega a cargar */
  .imagen-error {
    font-size: 0.9rem;
    line-height: 1.6;
    color: var(--color-texto-secundario);
  }

  /* ====================================================================
     6) ACCESIBILIDAD: respeta "reducir movimiento" del sistema
  ==================================================================== */
  @media (prefers-reduced-motion: reduce) {
    .btn-continuar,
    .imagen-wrapper {
      transition-duration: 0.01ms !important;
    }
  }

  /* ====================================================================
     7) RESPONSIVE: ajuste fino para pantallas muy pequeñas
  ==================================================================== */
  @media (max-width: 420px) {
    .btn-continuar {
      padding: 1rem 2.25rem;
    }
  }
</style>
</head>
<body>

  <!-- Botón principal: único elemento visible al cargar la página -->
  <button type="button" class="btn-continuar" id="btnContinuar">
    Continuar
  </button>

  <!-- Contenedor de la imagen: oculto hasta que se pulsa el botón -->
  <div class="imagen-wrapper" id="imagenWrapper">
    <!--
      Si "src" no enlaza DIRECTAMENTE a un archivo de imagen
      (.jpg, .png, .webp...), el navegador no podrá mostrarla y se
      verá el mensaje de respaldo de abajo. Sustituye la URL por la
      de tu propia imagen para personalizar este bloque.
    -->
    <img
      id="imagenRevelada"
      src="data:image/jpeg;base64,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
