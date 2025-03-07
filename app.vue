<template>
  <div>
    <Slider :slides="slideData" :loop="true" :autoplay="true" :autoplayDelay="5000" />
  </div>
</template>

<script>
import Slider from "@/components/Slider.vue";

export default {
  components: {
    Slider,
  },
  data() {
    return {
      slideData: [
        {
          title: "Slide 1",
          media: {
            type: "image",
            src: "https://cards.algoreducation.com/_next/image?url=https%3A%2F%2Ffiles.algoreducation.com%2Fproduction-ts%2F__S3__9691abed-ff1f-4b6b-aa55-e8acbc7366bb&w=3840&q=75",
          	w: "1080",
			h: "1920",
          
          },
        },
        {
          title: "Slide 2",
          media: {
            type: "image",
            src: "https://images.unsplash.com/photo-1494500764479-0c8f2919a3d8?fm=jpg&q=60&w=3000&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8NHx8cGFpc2FqZXxlbnwwfHwwfHx8MA%3D%3D",
          	w: "1080",
			h: "1920",
          
          },
        },
        {
          title: "Slide 3",
          media: {
            type: "image",
            src: "https://kaikucaffelatte.com/blog/wp-content/uploads/2020/03/shutterstock_510679489-scaled-e1648037045335.jpg",
          	w: "1080",
			h: "1920",
          
          
          },
        },

        {
          title: "Slide 4",
          media: {
            type: "image",
            src: "https://s1.significados.com/foto/paisaje-og.jpg",
            w: "1080",
            h: "1920",
          
          },
        },


        {
          title: "Slide 5",
          media: {
            type: "image",
            src: "https://us.123rf.com/450wm/angelarredondo/angelarredondo1812/angelarredondo181200066/136333600-preciosa-puesta-de-sol-de-finales-de-oto%C3%B1o-en-la-zona-de-sajambre-en-picos-de-europa-le%C3%B3n.jpg?ver=6",
          },
        },

        {
          title: "Slide 6",
          media: {
            type: "image",
            src: "https://i.pinimg.com/736x/a4/b2/d7/a4b2d7d9568da740e7fbb9a0dac9b1d7.jpg",
            w: "1080",
            h: "1920",
          },
        },

        {
          title: "Slide 7",
          media: {
            type: "image",
            src: "https://img.freepik.com/foto-gratis/atardecer_1048-4446.jpg",
            w: "1080",
            h: "1920",
          },
        },

      ],
    };
  },
};
</script>
