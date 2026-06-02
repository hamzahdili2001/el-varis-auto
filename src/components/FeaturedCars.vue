<!-- components/FeaturedCars.vue -->
<template>
    <v-container class="featured-container mt-10" fluid>
        <div class="section-heading">
            <h2 class="featured-title text-h4 mb-2 font-weight-bold">سيارات مميزة</h2>
            <p class="section-description">اكتشف أفضل السيارات المتاحة الآن، اختر ما يناسب رحلتك بأسلوب فاخر.</p>
        </div>

        <v-row class="featured-grid">
            <v-col v-for="car in cars" :key="car.id" cols="12" sm="6" md="3" lg="3">
                <v-card elevation="8" class="featured-card">
                    <v-img :src="car.image" height="220" cover class="featured-image" />

                    <v-card-text class="card-body">
                        <div class="card-title">{{ car.name }}</div>
                        <div class="card-subtitle">{{ car.tagline }}</div>
                    </v-card-text>

                    <v-card-actions class="card-actions">
                        <div class="price-tag">${{ car.price }} / يوم</div>
                        <v-btn class="rent-button" @click="openRent(car)">استئجار</v-btn>
                    </v-card-actions>
                </v-card>
            </v-col>
        </v-row>

        <!-- Rental dialog -->
        <v-dialog v-model="dialog" max-width="640">
            <v-card>
                <v-card-title>
                    <span class="text-h6">حجز: {{ selectedCar ? selectedCar.name : '' }}</span>
                </v-card-title>

                <v-card-text>
                    <v-form ref="formRef" lazy-validation>
                        <v-row>
                            <v-col cols="12" sm="6">
                                <v-text-field v-model="form.firstName" :rules="[rules.required]" label="الاسم الأول"
                                    outlined />
                            </v-col>
                            <v-col cols="12" sm="6">
                                <v-text-field v-model="form.lastName" :rules="[rules.required]" label="اسم العائلة"
                                    outlined />
                            </v-col>

                            <v-col cols="12" sm="6">
                                <v-text-field v-model="form.email" :rules="[rules.required, rules.email]"
                                    label="البريد الإلكتروني" type="email" outlined />
                            </v-col>

                            <v-col cols="12" sm="6">
                                <v-text-field v-model="form.phone" :rules="[rules.required, rules.phone]"
                                    label="رقم الهاتف" type="tel" outlined />
                            </v-col>

                            <v-col cols="12" sm="6">
                                <v-text-field v-model="form.startDate" :rules="[rules.required]" label="تاريخ الاستلام"
                                    type="date" outlined />
                            </v-col>

                            <v-col cols="12" sm="6">
                                <v-text-field v-model.number="form.days" :rules="[rules.required, rules.days]"
                                    label="عدد الأيام" type="number" min="1" outlined />
                            </v-col>
                        </v-row>
                    </v-form>
                </v-card-text>

                <v-card-actions>
                    <v-spacer />
                    <v-btn text @click="dialog = false">إلغاء</v-btn>
                    <v-btn color="primary" @click="submitRental">إرسال الطلب</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>

        <v-snackbar v-model="snackbar" timeout="4000">{{ snackbarMsg }}</v-snackbar>
    </v-container>
</template>

<script setup>
import { ref } from 'vue'

const cars = [
    { id: 1, name: 'BMW M4 Competition', tagline: 'أداء رياضي فائق', price: 120, image: '/cars/1.jpeg' },
    { id: 2, name: 'Audi RS7', tagline: 'قوة وأناقة بتحكم كامل', price: 150, image: '/cars/3.jpeg' },
    { id: 3, name: 'Mercedes AMG GT', tagline: 'فخامة وريادة على الطريق', price: 180, image: '/cars/2.jpeg' },
    { id: 4, name: 'Porsche 911 Carrera', tagline: 'سرعة مميزة وشخصية جريئة', price: 210, image: '/cars/4.jpeg' },
    { id: 5, name: 'Lamborghini Huracan', tagline: 'تجربة قيادة خارقة', price: 330, image: '/cars/5.jpeg' },
    { id: 6, name: 'Ferrari Roma', tagline: 'نمط حياة رياضي فاخر', price: 290, image: '/cars/1.jpeg' },
    { id: 7, name: 'Range Rover Sport', tagline: 'راحة وديناميكية في كل رحلة', price: 170, image: '/cars/4.jpeg' },
    { id: 8, name: 'Tesla Model S', tagline: 'قيادة كهربائية مستقبلية', price: 160, image: '/cars/2.jpeg' },
    { id: 9, name: 'Mercedes G-Class', tagline: 'قوة عصرية ورفاهية مطلقة', price: 220, image: '/cars/3.jpeg' },
    { id: 10, name: 'Audi Q8', tagline: 'رحابة وأناقة للطرق الطويلة', price: 140, image: '/cars/5.jpeg' },
    { id: 11, name: 'BMW X7', tagline: 'مساحة واسعة وأناقة فاخرة', price: 155, image: '/cars/2.jpeg' },
    { id: 12, name: 'Maserati Levante', tagline: 'تفرد إيطالي وأسلوب قوي', price: 195, image: '/cars/1.jpeg' }
]

const dialog = ref(false)
const snackbar = ref(false)
const snackbarMsg = ref('')
const selectedCar = ref(null)

const formRef = ref(null)
const form = ref({ firstName: '', lastName: '', email: '', phone: '', startDate: '', days: 1 })

const rules = {
    required: v => !!v || 'هذا الحقل مطلوب',
    email: v => /\S+@\S+\.\S+/.test(v) || 'بريد إلكتروني غير صالح',
    phone: v => (!!v && v.length >= 7) || 'رقم هاتف غير صالح',
    days: v => (v && v > 0) || 'المدة يجب أن تكون على الأقل يوم واحد'
}

function openRent(car) {
    selectedCar.value = car
    form.value = { firstName: '', lastName: '', email: '', phone: '', startDate: '', days: 1 }
    dialog.value = true
}

function submitRental() {
    if (formRef.value && typeof formRef.value.validate === 'function') {
        const valid = formRef.value.validate()
        if (!valid) return
    }

    console.log('Rental request', { car: selectedCar.value, ...form.value })
    snackbarMsg.value = 'تم إرسال طلب الحجز بنجاح'
    snackbar.value = true
    dialog.value = false
}
</script>

<style scoped>
.featured-container {
    width: 100%;
    max-width: 100%;
    padding: 0 16px;
}

.section-heading {
    text-align: center;
    max-width: 760px;
    margin: 0 auto 32px;
}

.section-description {
    color: #64748b;
    font-size: 1rem;
    line-height: 1.7;
    margin: 0 auto;
}

.featured-grid {
    gap: 24px;
}

.featured-card {
    border-radius: 24px;
    overflow: hidden;
    transition: transform 0.25s ease, box-shadow 0.25s ease;
    background: rgba(255, 255, 255, 0.92);
    border: 1px solid rgba(15, 23, 42, 0.08);
    color: #0f172a;
}

.featured-card:hover {
    transform: translateY(-10px);
    box-shadow: 0 28px 60px rgba(15, 23, 42, 0.12);
}

.featured-image {
    min-height: 220px;
}

.card-body {
    padding: 20px;
}

.card-title {
    font-size: 1.1rem;
    font-weight: 700;
    margin-bottom: 8px;
    color: #0f172a;
}

.card-subtitle {
    color: #64748b;
    font-size: 0.95rem;
}

.card-actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 18px 20px 20px;
    gap: 12px;
}

.price-tag {
    font-weight: 700;
    color: #0f172a;
}

.rent-button {
    min-width: 120px;
    background: #000000;
    color: #ffffff;
    border-radius: 10px;
    text-transform: none;
}

.rent-button:hover {
    background: #111111;
}

@media (max-width: 960px) {
    .section-description {
        font-size: 0.98rem;
    }
}

@media (max-width: 600px) {
    .card-actions {
        flex-direction: column;
        align-items: stretch;
    }

    .rent-button {
        width: 100%;
    }
}
</style>
