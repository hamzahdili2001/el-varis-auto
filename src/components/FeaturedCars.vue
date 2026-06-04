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

                        <div class="card-tags">
                            <v-chip size="small" variant="outlined" class="tag-chip">
                                {{ car.transmission === 'automatic' ? '🔄 automatique ' : '⚙️ manuel' }}
                            </v-chip>
                            <v-chip size="small" variant="outlined" class="tag-chip">
                                {{ car.fuel_type === 'diesel' ? '⛽ diesel' : '⛽ essence' }}
                            </v-chip>
                            <v-chip size="small" :variant="car.is_available ? 'tonal' : 'outlined'"
                                :color="car.is_available ? 'success' : 'error'" class="tag-chip">
                                {{ car.is_available ? '✓ متاح' : '✗ غير متاح' }}
                            </v-chip>
                        </div>
                    </v-card-text>

                    <v-card-actions class="card-actions">
                        <div class="price-tag">{{ car.price }} Dh / يوم</div>
                        <v-btn class="rent-button" @click="openBooking(car)"
                            :disabled="!car.is_available">استئجار</v-btn>
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

import { ref, onMounted, reactive } from 'vue'
import { supabase } from '../lib/supabase'

import emailjs from '@emailjs/browser'

const cars = ref([])

async function getCars() {
    const { data, error } = await supabase.from('cars').select('*')
    console.log('Supabase cars fetch result:', { data, error })

    if (error) {
        console.error('Supabase fetch error:', error)
        return
    }

    if (!data || data.length === 0) {
        console.warn('Supabase returned an empty array from cars table.')
    }

    cars.value = data || []
}

onMounted(() => {
    getCars()
})


// 1. UI Control States
const dialog = ref(false)
const snackbar = ref(false)
const snackbarMsg = ref('')
const formRef = ref(null)      // Ties directly to your <v-form ref="formRef">
const selectedCar = ref(null)  // Keeps track of which car the user is booking

// 2. Form Reactive Object (matches your v-models exactly)
const form = reactive({
    firstName: '',
    lastName: '',
    email: '',
    phone: '',
    startDate: '',
    days: 1
})

// 3. Arabic Validation Rules (matches your :rules)
const rules = {
    required: v => !!v || 'هذا الحقل مطلوب',
    email: v => /.+@.+\..+/.test(v) || 'البريد الإلكتروني غير صحيح',
    phone: v => /^[0-9+\s-]{8,15}$/.test(v) || 'رقم الهاتف غير صحيح',
    days: v => (v && v > 0) || 'يجب أن يكون عدد الأيام 1 أو أكثر'
}

// 4. Form Reset Helper
const resetForm = () => {
    if (formRef.value) formRef.value.reset()
    form.firstName = ''
    form.lastName = ''
    form.email = ''
    form.phone = ''
    form.startDate = ''
    form.days = 1
}

// 5. Submit Function (tied to your @click="submitRental")

const submitRental = async () => {
    const { valid } = await formRef.value.validate()
    if (!valid) return

    try {
        // REMOVE car_name from this object
        const bookingPayload = {
            car_id: selectedCar.value?.id, // This links perfectly to your cars table ID
            first_name: form.firstName,
            last_name: form.lastName,
            email: form.email,
            phone: form.phone,
            start_date: form.startDate,
            days: parseInt(form.days)
        }



        // Insert the row into your 'bookings' table
        const { data, error } = await supabase
            .from('bookings')
            .insert([ bookingPayload ])

        if (error) throw error

        // Step B: send confirmation email if possible, but don't fail the booking if email fails
        const emailParams = {
            title: "🚗 NEW EL VARIS AUTO",
            name: form.firstName + " " + form.lastName,
            first_name: form.firstName,
            last_name: form.lastName,
            phone: form.phone,
            start_date: form.startDate,
            email: form.email,
        }

        let bookingMessage = 'تم إرسال طلب الحجز بنجاح!'

        try {
            await emailjs.send(
                'service_s929lmw',   // Replace with your EmailJS Service ID
                'template_kiqz6ki',  // Replace with your EmailJS Template ID
                emailParams,
                'Jg4C3ybZVkofiomVn'    // Replace with your EmailJS Public Key
            )
        } catch (emailError) {
            console.error('EmailJS send failed:', emailError)
            bookingMessage = 'تم حفظ الطلب، ولكن لم يتم إرسال البريد الإلكتروني للتأكيد.'
        }

        snackbarMsg.value = bookingMessage
        snackbar.value = true
        dialog.value = false
        resetForm()

    } catch (error) {
        console.error('Error saving booking:', error)
        snackbarMsg.value = 'حدث خطأ أثناء إرسال الطلب: ' + (error.message || error)
        snackbar.value = true
    }
}


const openBooking = (car) => {
    selectedCar.value = car // Sets the car metadata (id, name) for the form title and payload
    dialog.value = true     // Opens the dialog
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

.card-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 12px;
}

.tag-chip {
    font-size: 0.85rem;
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
