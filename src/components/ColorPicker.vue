<script lang="ts">
import { defineComponent, ref } from "vue";
import axios from "axios";

// Define the props type outside of the component definition
interface Props {
  msg: string;
}

export default defineComponent({
  props: {
    msg: {
      type: String,
      required: true,
    }
    
  },
  setup(props: Props) {

  
interface Color {
    hex_code: string;
    color_code: string;
    name: string;
}

const arrColors = ref<Color[]>([]); // Change the type to Color[]
const selectedColor = ref<string | undefined>(undefined);
const colorName = ref<string | undefined>(undefined);
const colorCode = ref<string | undefined>(undefined);

    // Function to fetch colors
    const loadColor = () => {
      axios
        .get("https://api.prolook.com/api/colors/prolook")
        .then((response) => {
          arrColors.value = response.data.colors;
          console.log(arrColors.value);
        })
        .catch((error) => {
          console.error("Error fetching colors:", error);
        });
    };

    // Function to get color
    const getColor = (index: number): void => {
      const color = arrColors.value[index];
      if (color) {
        selectedColor.value = "#" + color.hex_code;
        colorCode.value = color.color_code;
        colorName.value = color.name;

        // Set the text color based on the background color
        const textColor = getContrastYIQ(color.hex_code);
        document.documentElement.style.setProperty("--text-color", textColor);
      }
    };

    const getContrastYIQ = (hexcolor: string) => {
      hexcolor = hexcolor.replace("#", "");
      const r = parseInt(hexcolor.substr(0, 2), 16);
      const g = parseInt(hexcolor.substr(2, 2), 16);
      const b = parseInt(hexcolor.substr(4, 2), 16);
      const brightness = (r * 299 + g * 587 + b * 114) / 255000;
      return brightness >= 0.5 ? "black" : "white";
    };

    // Call the function to fetch colors when the component is mounted
    loadColor();

    return {
      arrColors,
      loadColor,
      selectedColor,
      colorName,
      colorCode,
      getColor,
      getContrastYIQ,
      msg: props.msg, // Access msg from props
    };
  },
});
</script>

<template>
  <h1>{{ msg }}</h1>

  <v-card>
    <v-row>
      <v-col cols="6">
        <v-card dense class="mx-auto" width="500">
          <v-card-title class="left-align">Colors:</v-card-title>
          <v-card-text class="scrollable">
            <v-list dense lines="one">
              <v-list-item
                v-for="(item, i) in arrColors"
                :key="item.color_code"
                :title="item.name"
                class="left-align border"
              >
                <template v-slot:append>
                  <v-btn color="blue" @click="getColor(i)">Preview</v-btn>
                </template>
              </v-list-item>
            </v-list>
          </v-card-text>
        </v-card>
      </v-col>

      <v-col cols="6">
        <v-card dense class="mx-auto" width="500">
          <v-sheet
            class="d-flex"
            :color="selectedColor == '' 
            ? 'grey' 
            : selectedColor"
            height="250"
          >
            <v-text class="text-color ma-auto" v-if="selectedColor != ''">
              name: {{ colorName }} <br />
              hex code: {{ selectedColor }} <br />
              color code: {{ colorCode }}
            </v-text>
          </v-sheet>
        </v-card>
      </v-col>
    </v-row>
  </v-card>
</template>

<style scoped>
.text-color {
  color: var(
    --text-color,
    black
  ); /* Fallback to black if variable is not set */
}
.scrollable {
  max-height: 700px;
  overflow-y: auto;
}

/* Hide scrollbar for WebKit browsers */
.scrollable::-webkit-scrollbar {
  display: none;
}
.border {
  border: 1px solid #ccc;
}

.left-align {
  text-align: left;
}
</style>
