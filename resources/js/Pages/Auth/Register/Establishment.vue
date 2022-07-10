<script setup>
import BreezeButton from '@/Components/Button.vue';
import BreezeGuestLayout from '@/Layouts/Guest.vue';
import BreezeInput from '@/Components/Input.vue';
import BreezeLabel from '@/Components/Label.vue';
import BreezeValidationErrors from '@/Components/ValidationErrors.vue';
import { Head, Link, useForm } from '@inertiajs/inertia-vue3';
import { computed } from 'vue'

const activityAreas = [{
  slug: 'bedAndFood',
  label: 'Hôtellerie / Restauration',
  sms: [{
    slug: 'bedAndFood1',
    text: 'bedAndFood text 1',
  }, {
    slug: 'bedAndFood2',
    text: 'bedAndFood text 2',
  }, {
    slug: 'bedAndFood3',
    text: 'bedAndFood text 3',
  },]
},
{
  slug: 'healthCare',
  label: 'Esthétique / Coiffure / Beauté / Bien-être',
  sms: [{
    slug: 'healthCare1',
    text: 'healthCare text 1',
  }, {
    slug: 'healthCare2',
    text: 'healthCare text 2',
  }, {
    slug: 'healthCare3',
    text: 'healthCare text 3',
  },]
},
{
  slug: 'wellBeing',
  label: 'Médical / Soin',
  sms: [{
    slug: 'wellBeing1',
    text: 'wellBeing text 1',
  }, {
    slug: 'wellBeing2',
    text: 'wellBeing text 2',
  }, {
    slug: 'wellBeing3',
    text: 'wellBeing text 3',
  },]
},
{
  slug: 'other',
  label: 'Autre',
  sms: [{
    slug: 'other1',
    text: 'other text 1',
  }, {
    slug: 'other2',
    text: 'other text 2',
  }, {
    slug: 'other3',
    text: 'other text 3',
  },]
}]

const smsList = computed(() => {
  if(!form.activityArea) return

  const area = (activityAreas.filter((area) => area.slug === form.activityArea))[0]
  return area.sms
})

const form = useForm({
    activityArea: '',
    openingHours: [],
    bookingLink: '',
});

const submit = () => {
    form.post(route('register'), {
        onFinish: () => form.reset('password', 'password_confirmation'),
    });
};
</script>

<template>
    <BreezeGuestLayout>
        <Head title="Register" />

        <BreezeValidationErrors class="mb-4" />

        <form @submit.prevent="submit">
            <div>
                <BreezeLabel for="activityArea" value="Secteur d'activité" />
                <select name="activityArea" id="activityArea" v-model="form.activityArea">
                  <option 
                    v-for="activity in activityAreas" 
                    :key="activity.slug" 
                    :value="activity.slug"
                  >{{ activity.label }}</option>
                </select>
            </div>
            <div v-if="smsList">
                <BreezeLabel for="activityArea" value="Secteur d'activité" />
                <select name="activityArea" id="activityArea" v-model="form.sms">
                  <option 
                    v-for="sms in smsList" 
                    :key="sms.slug" 
                    :value="sms.slug"
                  >{{ sms.text }}</option>
                </select>
            </div>
            <div>
                <BreezeLabel for="openingHours" value="Heure d'ouvertures" />
                <textarea v-model="form.openingHours" />
            </div>
            
            <div class="mt-4">
                <BreezeLabel for="bookingLink" value="Lien de réservation" />
                <BreezeInput id="bookingLink" type="bookingLink" class="mt-1 block w-full" v-model="form.bookingLink" required autocomplete="bookingLink" />
            </div>

            <div class="flex items-center justify-end mt-4">
                <BreezeButton class="ml-4" :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                    Register
                </BreezeButton>
            </div>
        </form>
    </BreezeGuestLayout>
</template>
