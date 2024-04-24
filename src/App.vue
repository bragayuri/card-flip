<template>
  <div class="grid-container" ref="gridContainer">
    <div
      class="card"
      v-for="(item, index) in items"
      :key="index"
      :class="{ flipped: item.flipped }"
    >
      <div @click="flipCard(index, $event)" class="card-front">
        <img :src="item.image" alt="Dish Image" />
        <p class="card-number">{{ index + 1 }}</p>
      </div>
      <div class="card-back">
        <div class="content">
          <ScratchCard
            :id="index"
            :width="300"
            :height="50"
            @scratching="handleScratching"
            @contentRevealed="triggerConfetti"
          >
            <template v-slot>
              <b class="card-person" :class="item.person">
                {{ item.person }}'s favourite dish
              </b>
            </template>
          </ScratchCard>
          <p class="card-food">{{ item.name }}</p>
          <p class="card-description">{{ item.description }}</p>
          <p class="card-question">{{ item.question }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import confetti from "canvas-confetti";
import ScratchCard from "./components/ScratchCard.vue";

export default {
  name: "FlipCardGame",
  components: { ScratchCard },
  data() {
    return {
      isScratching: false,
      items: [
        {
          person: "Seb",
          image:
            "https://s3.eu-west-2.amazonaws.com/make-it-scotch/images/Recipes/_recipeImageLarge/Pork-Milanese-4-600x580-1.jpg",
          name: "Milanese with tagliatelle",
          description:
            "A crispy, breaded veal cutlet served alongside buttery tagliatelle pasta, often garnished with a squeeze of fresh lemon and Parmesan.",
          question: "What’s your favorite comfort food?",
          flipped: false,
        },
        {
          person: "Yuri",
          image:
            "https://www.estadao.com.br/resizer/v2/DJIDV2SGYBN6DHNJMG64RJOIEE.jpg?quality=80&auth=59c06079f4d2bfc20f4cf6331fec5526ace076276873c8edc12d12c95513e1eb&width=720&height=503&focal=0,0",
          name: "Esfihas",
          description:
            "Esfihas, originate from the Middle East, particularly from Lebanon and Syria. A soft oven baked dough topped with meat or zatar.",
          question:
            "Are there any foods or ingredients that you personally avoid or recommend avoiding?",
          flipped: false,
        },
        {
          person: "Arleta",
          image:
            "https://omnivorescookbook.com/wp-content/uploads/2022/05/220510_Mapo-Tofu_550.jpg",
          name: "Mapo Tofu",
          description:
            "Mapo Tofu is a fiery and flavorful Sichuan dish made with soft tofu in a spicy, pungent sauce with minced meat.",
          question: "Do you have any food allergies or dietary restrictions?",
          flipped: false,
        },
        {
          person: "Andrew",
          image:
            "https://images.immediate.co.uk/production/volatile/sites/30/2020/08/the-best-spaghetti-bolognese-7e83155.jpg",
          name: "Home made Bolognese",
          description:
            "A rich and hearty Italian meat sauce slow-cooked with tomatoes, minced beef, onions, carrots, celery, and red wine, traditionally served over pasta for a comforting meal.",
          question:
            "If you could pick that last meal of your life, what would that be (cripy I know)?",
          flipped: false,
        },
        {
          person: "Fidel",
          image:
            "https://guardian.ng/wp-content/uploads/2019/09/ukwa-african-breadfruit-dish-1280x720.jpeg",
          name: "Ukwa",
          description:
            "Ukwa is a Nigerian dish made from boiled or roasted African breadfruit seeds, often flavored with palm oil and pepper.",
          question: "What’s the most unusual food you’ve ever tried?",
          flipped: false,
        },
        {
          person: "Alia",
          image:
            "https://www.foodandwine.com/thmb/Wd4lBRZz3X_8qBr69UOu2m7I2iw=/1500x0/filters:no_upscale():max_bytes(150000):strip_icc()/classic-cheese-pizza-FT-RECIPE0422-31a2c938fc2546c9a07b7011658cfd05.jpg",
          name: "Pizza",
          description:
            "Italian dish consisting of a round, flat base of leavened wheat-based dough topped with tomatoes, cheese, and various other ingredients, baked at a high temperature, traditionally in a wood-fired oven.",
          question: "Sweet, savory, or spicy food?",
          flipped: false,
        },
        {
          person: "Hussain",
          image:
            "https://omnivorescookbook.com/wp-content/uploads/2023/05/230413_Chinese-Chicken-Dumplings_550.jpg",
          name: "Chicken Dumpling",
          description:
            "Chicken dumplings, featuring minced chicken wrapped in dough and then steamed, boiled, or fried.",
          question: "Any food you used to hate and now you love?",
          flipped: false,
        },
        {
          person: "Lucas",
          image:
            "https://images.immediate.co.uk/production/volatile/sites/30/2023/06/Next-level-Spanish-tortilla-1a79751.jpg?quality=90&resize=556,505",
          name: "Spanish Tortilla",
          description:
            "A Spanish tortilla, also known as tortilla española, is a thick omelette made primarily from eggs and potatoes, often including onions and occasionally other vegetables or chorizo.",
          question:
            "What’s the most memorable meal you’ve shared with friends or family?",
          flipped: false,
        },
        {
          person: "Dimitri",
          image:
            "https://www.allrecipes.com/thmb/YDL6yQbN_r59nTJenOx1q9OPdLs=/1500x0/filters:no_upscale():max_bytes(150000):strip_icc()/23852-creamy-chicken-and-wild-rice-soup-ddmfs-step1-08-4x3-6e07237694f44ec1a6f92610a56a241b.jpg",
          name: "Chicken Soup",
          description:
            "The person described this soup as an original so we rather ask him the description",
          question:
            "Have you ever been surprised by liking a food you thought you’d hate?",
          flipped: false,
        },
        {
          person: "Shazia",
          image:
            "https://handletheheat.com/wp-content/uploads/2020/10/BAKERY-STYLE-CHOCOLATE-CHIP-COOKIES-9-637x637-1.jpg",
          name: "Cookies",
          description:
            "Sweet, baked treats made from a dough of flour, sugar, and fat, often mixed with ingredients like chocolate chips, nuts, or dried fruit, and baked until golden and slightly crisp.",
          question: "What’s your favorite ice cream flavor?",
          flipped: false,
        },
        {
          person: "Diego C",
          image:
            "https://www.comedera.com/wp-content/uploads/2022/12/Pastel-de-choclo-chileno.jpg",
          name: "Pastel de Choclo",
          description:
            "Pastel de Choclo is a traditional Chilean dish that combines elements of a corn pie and a meat casserole.",
          question: "What is your favorite dish from your childhood?",
          flipped: false,
        },
        {
          person: "Diego Ipynb",
          image:
            "https://frommybowl.com/wp-content/uploads/2019/03/Lomo_Saltado_Vegan_GlutenFree_FromMyBowl-3.jpg",
          name: "Vegetarian Lomo Saltado dish",
          description:
            "Vegetarian Lomo Saltado is a meat-free version of the classic Peruvian stir-fry dish traditionally made with beef.",
          question:
            "Have you ever applied your data science skills to solve a challenging problem in the kitchen, and what was the outcome?",
          flipped: false,
        },
        {
          person: "Mustafa",
          image:
            "https://www.dirtyapronrecipes.com/wp-content/uploads/2019/02/nihari.jpg",
          name: "Beef Nihari",
          description:
            "Beef Nihari is a slow-cooked Pakistani stew traditionally made with beef shank, known for its rich, spicy broth.",
          question: "What’s the most adventurous thing you’ve done food-wise?",
          flipped: false,
        },
        {
          person: "Selin",
          image:
            "https://cdn77-s3.lazycatkitchen.com/wp-content/uploads/2021/03/vegan-turkish-pizza-lahmacun-1000x1500.jpg",
          name: "Lahmacun (turkish pizza)",
          description:
            "Lahmacun, also known as Turkish pizza, is a thin flatbread topped with minced meat (usually beef or lamb), vegetables, and herbs like onions, tomatoes, and parsley, seasoned with spices such as paprika and cumin.",
          flipped: false,
          question: "Do you have any special food traditions?",
        },
        {
          person: "Kyro",
          image:
            "https://img.taste.com.au/O2R_hleG/w720-h480-cfill-q80/taste/2021/12/airfryer-pavlova-175574-1.jpg",
          name: "Pavalova",
          description:
            "Pavlova is a meringue-based dessert named after the Russian ballerina Anna Pavlova.",
          question: "What’s your favorite food-themed movie or TV show?",
          flipped: false,
        },
        {
          person: "Nader",
          image:
            "https://petersfoodadventures.com/wp-content/uploads/2015/12/chicken-stroganoff-recipe.jpg",
          name: "Chicken Stroganoff",
          description:
            "Chicken Stroganoff is a variation of the classic Russian dish traditionally made with beef.",
          flipped: false,
          question:
            "Any food you love so much you end up eating every week or month?",
        },
        {
          person: "Arbaz",
          image:
            "https://www.indianhealthyrecipes.com/wp-content/uploads/2021/12/chicken-lollipop.jpg.webp",
          name: "Chicken lollipops",
          description:
            "Chicken lollipops are made from the middle (and sometimes inner) segments of chicken wings, shaped to resemble lollipops.",
          flipped: false,
          question: "What’s your go-to quick meal on a busy day?",
        },
        {
          person: "Divya",
          image:
            "https://www.cookwithmanali.com/wp-content/uploads/2020/05/Masala-Dosa.jpg",
          name: "Dosa with Chutney and sambar",
          description:
            "Dosa with Chutney and Sambar is a quintessential South Indian meal, celebrated for its delightful combination of flavors and textures.",
          flipped: false,
          question: "What food reminds you of home?",
        },
        {
          person: "Niklas",
          image:
            "https://assets.bonappetit.com/photos/57ad50c31b334044149755e7/master/pass/herb-crusted-rack-of-lamb-with-new-potatoes.jpg",
          name: "Herb Crusted Rack of Lamb",
          description:
            "A succulent roasted dish featuring a tender cut of lamb encased in a flavorful herb and breadcrumb crust.",
          flipped: false,
          question: "What’s the weirdest food combination you enjoy?",
        },
        {
          person: "Mazen",
          image:
            "https://plantbasedfolk.com/wp-content/uploads/2021/02/Vegan-Okra-with-Rice.jpg",
          name: "Bemye w Riz",
          description:
            "Fried okra is simmered in a flavored garlic tomato sauce with beef stew cubes and served on a bed of white rice with vermicelli noodles.",
          flipped: false,
          question: "If you could pick a dessert you love, what would that be?",
        },
        {
          person: "Jad",
          image:
            "https://amazingfoodanddrink.com/wp-content/uploads/2023/12/Molokhia.jpg",
          name: "Mlokhiyeh",
          description:
            "Mlokhiyeh is a traditional Middle Eastern dish made from jute leaves cooked with garlic, coriander, and chicken, served with rice or bread.",
          flipped: false,
          question: "Can you tell me an unforgettable experience with food?",
        },
        {
          person: "David",
          image:
            "https://www.cookwithmanali.com/wp-content/uploads/2021/04/Vegan-Sushi-500x500.jpg",
          name: "Sushi",
          question:
            "Any national dish you are proud to say it's from your birth country?",
          description:
            "Japanese dish featuring seasoned rice paired with a variety of ingredients such as raw fish, seafood, and vegetables. It's typically served with soy sauce, wasabi, and pickled ginger.",
        },
        {
          person: "Tarek",
          image:
            "https://bittmanproject.com/wp-content/uploads/ScreenShot2019-06-10at4.39.07PM.png",
          name: "Steak",
          question: "Any food you are not into it but people love?",
          description:
            "High-quality beef taken from the hindquarters of the animal, typically cut into thick slices that are cooked by grilling or frying.",
        },
        {
          person: "Kashish",
          image:
            "https://madhurasrecipe.com/wp-content/uploads/2023/05/Chole-Bhature-Featured.jpg",
          name: "Amritsari Chole Bhature",
          question: "Do you look at Food design when eating?",
          description:
            "Punjabi dish from Amritsar, India, featuring spicy curried chickpeas (Chole) paired with fluffy, deep-fried bread (Bhature).",
        },
        {
          person: "Ibtisam Tahir",
          image:
            "https://glebekitchen.com/wp-content/uploads/2019/12/chickenbiryanibowltop.jpg",
          name: "Biryani",
          question:
            "Have you ever had any bad experience with food?What happened?",
          description:
            "Biryani is a mixed rice dish, mainly popular in South Asia. It is made with rice, some type of meat and spices.",
        },
        {
          person: "Priyanka",
          image:
            "https://thecozycook.com/wp-content/uploads/2023/02/Homemade-Ramen-f.jpg",
          name: "Ramen",
          question: "What’s your favorite comfort food?",
          description:
            "Ramen is a Japanese noodle soup dish. It consists of Chinese-style wheat noodles served in a meat or fish-based broth, often flavored with soy sauce or miso.",
        },
        {
          person: "Samantha",
          image: "https://i.ibb.co/ysXvDXq/image-1.png",
          name: "Ham Hock Curry",
          question: "Do you often cook your own meals or buy it ready to eat?",
          description:
            "A family original so unique that we could not find a general picture online.",
        },
        {
          person: "Isa",
          question: "Can you tell us a dinner you will never forget?",
          image:
            "https://www.kawalingpinoy.com/wp-content/uploads/2018/06/filipino-style-pork-bbq-1.jpg",
          name: "Pork BBQ",
          description:
            "Pork BBQ is a succulent dish featuring tender pork slices marinated in a blend of soy sauce, garlic, sugar, and spices, typically grilled over an open flame for a smoky flavor.",
        },
      ],
    };
  },
  methods: {
    handleScratching(scratching) {
      this.isScratching = scratching;
    },
    flipCard(index, event) {
      if (!this.isScratching) {
        this.items[index].flipped = !this.items[index].flipped;
      }
    },
    triggerConfetti(index) {
      const card = this.$refs.gridContainer.children[index];
      const rect = card.getBoundingClientRect();

      const confettiOrigin = {
        x: (rect.left + rect.width / 2) / window.innerWidth,
        y: (rect.top + rect.height / 2) / window.innerHeight,
      };

      confetti({
        angle: 90,
        spread: 360,
        particleCount: 100,
        origin: confettiOrigin,
      });
    },
  },
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow-x: hidden;
}

.heading {
  margin: 0;
  font-size: 1.5rem;
  font-weight: bold;
  color: white;
  background-color: transparent;
  padding-inline-start: 1rem;
  padding-top: 2rem;
  background-color: black;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  padding: 1rem;
  gap: 20px;
  background-color: black;
}

.card {
  width: 100%;
  height: 300px;
  perspective: 1000px;
  position: relative;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1), 0 6px 20px rgba(0, 0, 0, 0.19);
  transition: transform 0.3s, box-shadow 0.3s;
  border-radius: 12px;
  background-size: cover;
  background-repeat: no-repeat;
  position: relative;
}

.card:hover {
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2), 0 12px 40px rgba(0, 0, 0, 0.25);
  transform: scale(1.05);
}

.card-number {
  position: absolute;
  background-color: yellow;
  box-shadow: 0px 4px 3px 0px rgba(255, 255, 255, 0.11) inset;
  top: 0;
  left: 1rem;
  font-size: 1rem;
  border-radius: 999px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 25px;
  height: 25px;
}

.card-front,
.card-back {
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  position: absolute;
  transition: transform 0.6s;
  display: flex;
  align-items: center;
  text-align: start;
  cursor: pointer;
}

.card-front {
  transform: rotateY(0deg);
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-back {
  transform: rotateY(180deg);
  flex-direction: column;
  justify-content: space-between;
  background-color: #fff;
  padding: 1rem 0;
}

.content {
  width: 90%;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: start;
  padding: 0.5rem;
}

.card-back p:first-of-type {
  flex-wrap: wrap;
  border-bottom: 1px solid black;
  width: 100%;
}

.card-person {
  font-weight: 600;
}

.card-food {
  font-weight: 650;
  width: 100%;
}

.card.flipped .card-front {
  transform: rotateY(-180deg);
}

.card.flipped .card-back {
  transform: rotateY(0deg);
}

@media (max-width: 1024px) {
  .grid-container {
    grid-template-columns: repeat(3, 1fr); /* Change to 3 columns on tablets */
  }
}

@media (max-width: 768px) {
  .grid-container {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 480px) {
  /* Mobile devices */
  .grid-container {
    grid-template-columns: 1fr;
    padding: 0.5rem;
    gap: 10px;
  }

  .card {
    height: 200px;
  }

  .heading {
    font-size: 1.2rem;
    padding-inline-start: 0.5rem;
    padding-top: 1rem;
  }

  .card-number {
    top: 10px;
    left: 10px;
    width: 20px;
    height: 20px;
  }
}
</style>
