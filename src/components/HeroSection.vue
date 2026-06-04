<!-- components/HeroSection.vue -->

<template>
    <div class="hero d-flex align-center justify-center text-center">
        <v-container>
            <h1 class="hero-title display-2 font-weight-bold mb-4">
                <span class="hero-brand">El Varis Auto</span>
            </h1>
            <p class="text-h5 mb-6">
                اختر من بين مجموعة واسعة من السيارات الفاخرة والرياضية لمغامرتك المقبلة
            </p>

            <div class="hero-actions">
                <v-btn class="whatsapp-btn" color="success" href="https://wa.me/212664162516" target="_blank"
                    rel="noopener">
                    <v-icon class="whatsapp-icon">mdi-whatsapp</v-icon>
                    0664162516
                </v-btn>
                <v-btn class="whatsapp-btn" color="success" href="https://wa.me/212664064306" target="_blank"
                    rel="noopener">
                    <v-icon class="whatsapp-icon">mdi-whatsapp</v-icon>
                    0664064306
                </v-btn>
            </div>
        </v-container>
    </div>
</template>




<style scoped>
.hero {
    min-height: 80vh;
    background: linear-gradient(135deg, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('/image.png');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    color: white;
    padding: 60px 0;
    text-align: center;
}

.hero-title {
    font-size: 5rem;
    line-height: 1.05;
}

.hero-brand {
    display: block;
    font-family: 'Brush Script MT', 'Lucida Handwriting', cursive;
    font-size: 5rem;
    letter-spacing: 0.12em;
    color: #ffd54f;
    text-shadow: 0 6px 28px rgba(0, 0, 0, 0.45);
    margin-bottom: 0.5rem;
}



.hero-actions {
    display: inline-flex;
    flex-wrap: wrap;
    gap: 16px;
    justify-content: center;
}

.whatsapp-icon {
    margin-inline-end: 8px;
    font-size: 1.3rem;
}

.whatsapp-btn {
    min-width: 150px;
    padding: 10px 20px;
    opacity: 0.75;
    transition: opacity 0.2s ease, transform 0.2s ease;
}

.whatsapp-btn:hover {
    opacity: 1;
    transform: translateY(-2px);
}

@media (max-width: 600px) {
    .hero {
        min-height: 70vh;
        padding: 40px 16px;
    }

    .hero-title {
        font-size: 2.75rem;
        line-height: 1.1;
    }

    .hero-brand {
        font-size: 2.4rem;
    }

    .hero-subtitle {
        font-size: 1rem;
    }

    .hero p {
        font-size: 1rem;
        max-width: 90%;
        margin: 0 auto 24px;
    }

    .hero-actions {
        width: 100%;
        flex-direction: column;
        gap: 12px;
    }

    .whatsapp-btn {
        width: 100%;
        min-width: auto;
        justify-content: center;
    }

    .whatsapp-icon {
        margin-inline-end: 6px;
        font-size: 1.1rem;
    }
}
</style>