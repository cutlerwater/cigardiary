<template>
  <div class="max-w-screen-md mx-auto px-4 py-10">
    <!-- Status Message -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 bg-light-grey rounded-md shadow-lg"
    >
    <p class="text-at-light-green">
        {{ statusMsg }}
      </p>
      <p class="text-red-500">{{ errorMsg }}</p>
      </div>

      <!-- Create -->
      <div class="p-8 flex items-start bg-natural rounded-md shadow-lg">
        <form class="flex flex-col gap-y-5 w-full">

          <h1 class="text-2xl text-at-red">Record cigar</h1>
          <!-- Cigar name -->
          <div class="flex flex-col">
            <label for="cigar-name" class="mb-1 text-sm text-at-red">Cigar Name</label>
            <input type="text" required class="p-2 text-gray-500 focus:outline-none" id="cigar-name" v-model="cigar_Name">
          </div>
          <!-- cigar brand -->
          <div class="flex flex-col">
              <label for="cigar_brand" class="mb-1 text-sm text-at-red">Cigar Brand</label>
              <select id="cigar_brand" class="p-2 text-gray-500 focus:outline-none" required  vmodel="cigar_brand">
                <option value="select-workout">Select Brand</option>
                  <option value="arturofuente">Arturo Fuente</option>
                  <option value="ashton">Ashton</option>
                  <option value="cohibad">Cohiba (Dominican)</option>
                  <option value="davidoff">Davidoff</option>
                  <option value="macanudo">Macanudo</option>
                  <option value="montecristo">Montecristo</option>
                  <option value="Oliva">Oliva</option>
                  <option value="padron">Padron</option>
                  <option value="perdomo">Perdomo</option>
                  <option value="rockypatel">Rocky Patel</option>                  
              </select>
          </div>
           <!-- Cigar Inputs -->
        <div class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in cigars"
            :key="index"
          >
            <div class="flex flex-col flex-1">
              <label for="strength" class="mb-1 text-sm text-at-light-green">Strength</label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.strength"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="size" class="mb-1 text-sm text-at-light-green">Size </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.size"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="cigar_shape" class="mb-1 text-sm text-at-light-green"
                >Shape
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.cigar_shape"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="ring_gauge" class="mb-1 text-sm text-at-light-green"
                >Ring Gauge
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.ring_gauge"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="country" class="mb-1 text-sm text-at-light-green"
                >Country of Origin
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.country"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="filler" class="mb-1 text-sm text-at-light-green"
                >Filler
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.filler"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="wrapper" class="mb-1 text-sm text-at-red"
                >Wrapper
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.wrapper"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="price" class="mb-1 text-sm text-at-red"
                >Price
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.price"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="rating" class="mb-1 text-sm text-at-red"
                >Rating
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.rating"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="Review" class="mb-1 text-sm text-at-red"
                >Review
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.review"
              />
            </div>
            <img
              @click="deleteCigar(item.id)"
              src="@/assets/images/trash-light-green.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt=""
            />
          </div>
          <button
            @click="addCigar"
            type="button"
            class="mt-6 py-2 px-6 rounded-sm self-start text-sm
      text-white bg-at-green duration-200 border-solid
      border-2 border-transparent hover:border-at-light-green hover:bg-white
      hover:text-at-light-green"
          >
            Add Cigar
          </button>
        </div>

        

        <button
          type="submit"
          class="mt-6 py-2 px-6 rounded-sm self-start text-sm
      text-white bg-at-green duration-200 border-solid
      border-2 border-transparent hover:border-at-light-green hover:bg-white
      hover:text-at-light-green"
        >
          Record Cigar
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { uid } from "uid";
import { supabase } from "../supabase/init";
// import { supabase } from "../supabase/init";

export default {
  name: "create",
  setup() {
    // Create data
    const cigar_Name = ref("");
    const cigar_Brand = ref("select brand");
    const cigars = ref([]);
    
    const statusMsg = ref(null);
    const errorMsg = ref(null);
    // Add cigar
    const addCigar = () => {
      cigars.value.push({
          id: uid(),
          cigar: "",
          strength: "",
          size: "",
          shape: "",
          ring_gauge: "",
          country: "",
          filler: "",
          wrapper: "",
          price: "",
          rating: "",
          review: "",
        });
        return;
      }
      
    
    // Delete cigar
     const deleteCigar= (id) => {
      if (cigars.value.length > 1) {
        cigars.value = cigars.value.filter((cigar) => cigar.id !== id);
        return;
      }
      errorMsg.value = "Error: Cannot remove, need to at least have one cigar";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };

    const createCigar = async () => {
      try {
        const { error } = await supabase.from("cigars").insert([
          {
            cigar_Name: cigar_Name.value,
            cigar_Brand: cigar_Brand.value,
            cigars: cigars.value,
          },
        ]);
        if (error) throw error;
        statusMsg.value = "Succes: Cigar Created!";
        cigar_Name.value = null;
        cigar_Brand.value = "select-brand";
        cigars.value = [];
        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };
    // Listens for chaging of cigar type input
    // Create cigar 
    return {cigar_Name, cigar_Brand, cigars, statusMsg, errorMsg, addCigar, deleteCigar, createCigar, };
  },
};
</script>