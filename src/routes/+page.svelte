<script>
  // https://www.youtube.com/watch?v=6QE8dXq9SOE&t=1s

  //thanks to CODINGNEPAL for the original vanilla JS version

  // npm install --save @fortawesome/fontawesome-free

  import "../app.css";
  import { onMount } from "svelte";

  let carousel,
    wrapper,
    firstCardWidth,
    timeoutId,
    isAutoPlay = true,
    reStartAutoPlay;

  //onMount used to pick out DOM elements
  onMount(() => {
    wrapper = document.querySelector(".wrapper");
    carousel = document.querySelector(".carousel");
    firstCardWidth = carousel.querySelector(".card").offsetWidth;
    const carouselChildren = [...carousel.children];

    //get number of cards that can fit in the carousel at once
    let cardsPerView = Math.round(carousel.offsetWidth / firstCardWidth);
    //insert copies of the last few cards to start of carousel for infinite scrolling
    carouselChildren
      .slice(-cardsPerView)
      .reverse()
      .forEach(card => {
        carousel.insertAdjacentHTML("afterbegin", card.outerHTML);
      });
    //insert copies of the first few cards to end of carousel for infinite scrolling
    carouselChildren.slice(0, cardsPerView).forEach(card => {
      carousel.insertAdjacentHTML("beforeend", card.outerHTML);
    });

    // Scroll the carousel at appropriate postition to hide first few duplicate cards on Firefox/chrome etc
    carousel.scrollLeft =
      carousel.offsetWidth + (1.75 * carousel.offsetWidth) / firstCardWidth;

    const autoPlay = () => {
      carousel.classList.remove("dragging");
      if (window.innerWidth < 800 || !isAutoPlay) return; // Return if window is smaller than 800 or isAutoPlay is false
      // Autoplay the carousel after every 2500 ms
      timeoutId = setInterval(() => {
        carousel.scrollLeft += firstCardWidth;
      }, 2500);
    };
    autoPlay();

    reStartAutoPlay = () => {
      autoPlay();
    };

    clearInterval(timeoutId);
    if (!wrapper.matches(":hover")) {
      autoPlay();
    }
  });

  let isDragging = false,
    startX,
    startScrollLeft;

  const dragStart = e => {
    isDragging = true;
    carousel.classList.add("dragging");
    carousel.style = "cursor:grab; user-select: none;";
    //records the initial cursor and scroll position of the carousel
    startX = e.pageX;
    startScrollLeft = carousel.scrollLeft;
  };

  const dragging = e => {
    if (!isDragging) return;
    //updates the scroll position of the carousel based on the cursor movement/direction
    carousel.scrollLeft = startScrollLeft - (e.pageX - startX);
  };

  const dragStop = () => {
    isDragging = false;
    carousel.classList.remove("dragging");
    document.querySelector(".carousel .card").style =
      "scroll-snap-align: start";
  };

  const btnScroll = e => {
    const btnId = e.srcElement.attributes.id.value;
    carousel.scrollLeft += btnId === "left" ? -firstCardWidth : firstCardWidth;
    carousel.style = "scroll-behavior: smooth";
  };

  const infiniteScroll = () => {
    //if carousel is at beginning, scroll to end
    if (carousel.scrollLeft === 0) {
      //instead of fiddling about with classes, this method is far easier
      carousel.style = "scroll-behavior: auto";
      carousel.scrollLeft = carousel.scrollWidth - 2 * carousel.offsetWidth;
      carousel.style = "scroll-behavior: smooth";
    } //if carousel is at end, scroll to beginning
    else if (
      Math.ceil(carousel.scrollLeft) ===
      carousel.scrollWidth - carousel.offsetWidth
    ) {
      carousel.style = "scroll-behavior: auto;";
      carousel.scrollLeft = carousel.offsetWidth;
      carousel.style = "scroll-behavior: smooth;";
    }
  };

  const imgsArr = Object.keys(import.meta.glob("$lib/images/**/*.*"));

  let cardsArray = [
    {
      id: 1,
      title: "Day Trips",
      subTitle: "Out amongst the Lions",
      details:
        "Donec accumsan <strong>ullamcorper</strong> diam nec finibus. Etiam et ante justo. Cras ac augue fringilla, <strong>hendrerit</strong> dolor ut, porta nisi.<br><br>Fusce <strong>tempor</strong> enim at commodo volutpat. Nullam vehicula, sapien quis eleifend vestibulum, <strong>neque</strong> nunc pharetra nisl, eget interdum sem felis vitae ex. ",
      image: imgsArr[0],
    },
    {
      id: 2,
      title: "Space Travel",
      subTitle: "Bathe amid the Stars",
      details:
        "Curabitur quis dictum nibh, <strong>mattis</strong> iaculis nibh. In hac habitasse platea dictumst. Nam <strong>accumsan</strong> libero non enim euismod, a <strong>tincidunt</strong> libero blandit. Mauris sit <strong>amet</strong> imperdiet tellus, vel fringilla <strong>lorem</strong>. Sed suscipit lacus et orci placerat blandit. <br><br>Integer pulvinar <strong>magna</strong> eros, quis ullamcorper lectus aliquam vitae. Mauris id nulla blandit. Curabitur quis dictum nibh, mattis iaculis nibh.",
      image: imgsArr[1],
    },
    {
      id: 3,
      title: "Run Wild",
      subTitle: "Beat the Retreat",
      details:
        "Mauris non <strong>tellus</strong> vulputate, feugiat erat non, pharetra justo. Proin consequat <strong>felis diam</strong>, malesuada auctor mi convallis sed. Sed <strong>gravida</strong> faucibus vulputate. Mauris mauris <strong>tortor</strong>,  Praesent sit amet <strong>ipsum</strong> eu risus varius blandit. <br><br>Donec blandit ac lorem et dignissim. Nam sem nisl, aliquam a ornare eu, luctus <strong>quis enim</strong>. Aenean ut felis in neque congue rutrum. Aliquam <strong>at</strong> velit.",
      image: imgsArr[2],
    },
    {
      id: 4,
      title: "Stay Home",
      subTitle: "Behind the fridge",
      details:
        "Lorem ipsum <strong>dolor</strong> sit amet, consectetur adipiscing elit, <strong>sed</strong> do eiusmod tempor incididunt ut labore et <strong>dolore</strong> magna aliqua. Mauris rhoncus <strong>aenean</strong> vel elit. Nibh sed pulvinar proin gravida hendrerit lectus a. <strong>Semper risus</strong> in hendrerit gravida rutrum quisque non tellus.<br><br>Mi <strong>ipsum</strong> faucibus vitae aliquet nec ullamcorper sit amet. Habitant <strong>morbi</strong> tristique senectus et netus et.Faucibus turpis in eu  <strong>mi </strong> bibendum neque <strong>egestas</strong> congue quisque.",
      image: imgsArr[3],
    },
    {
      id: 5,
      title: "Research",
      subTitle: "Taming the shrews",
      details:
        "Donec accumsan <strong>ullamcorper</strong> diam nec finibus. Etiam et ante justo. Cras ac augue fringilla, <strong>hendrerit</strong> dolor ut, porta nisi.<br><br>Fusce <strong>tempor</strong> enim at commodo volutpat. Nullam vehicula, sapien quis eleifend vestibulum, <strong>neque</strong> nunc pharetra nisl, eget interdum sem felis vitae ex. ",
      image: imgsArr[4],
    },
    {
      id: 6,
      title: "Time Men",
      subTitle: "Bathe amid the Stars",
      details:
        "Curabitur quis dictum nibh, <strong>mattis</strong> iaculis nibh. In hac habitasse platea dictumst. Nam <strong>accumsan</strong> libero non enim euismod, a <strong>tincidunt</strong> libero blandit. Mauris sit <strong>amet</strong> imperdiet tellus, vel fringilla <strong>lorem</strong>. Sed suscipit lacus et orci placerat blandit. <br><br>Integer pulvinar <strong>magna</strong> eros, quis ullamcorper lectus aliquam vitae. Mauris id nulla blandit. Curabitur quis dictum nibh, mattis iaculis nibh.",
      image: imgsArr[0],
    },
    {
      id: 7,
      title: "Inca Roads",
      subTitle: "Beat the Retreat",
      details:
        "Mauris non <strong>tellus</strong> vulputate, feugiat erat non, pharetra justo. Proin consequat <strong>felis diam</strong>, malesuada auctor mi convallis sed. Sed <strong>gravida</strong> faucibus vulputate. Mauris mauris <strong>tortor</strong>,  Praesent sit amet <strong>ipsum</strong> eu risus varius blandit. <br><br>Donec blandit ac lorem et dignissim. Nam sem nisl, aliquam a ornare eu, luctus <strong>quis enim</strong>. Aenean ut felis in neque congue rutrum. Aliquam <strong>at</strong> velit.",
      image: imgsArr[1],
    },
  ];
</script>

<svelte:window on:mouseup={dragStop} />

<div
  class="wrapper"
  on:mouseenter={() => clearInterval(timeoutId)}
  on:mouseleave={reStartAutoPlay}
>
  <i
    id="left"
    class="fa-solid fa-angle-left"
    on:click={btnScroll}
    on:keydown={btnScroll}
  />

  <!-- dragging class required below to prevent errors as it doesn't exist until it's injected in by the dragStart function -->
  <div
    bind:this={carousel}
    class="carousel dragging"
    on:mousemove={dragging}
    on:mousedown={dragStart}
    on:scroll={infiniteScroll}
  >
    {#each cardsArray as card}
      <div class="card">
        <div class="img"
          ><img src={card.image} alt={card.title} draggable="false" /></div
        >
        <h2>{card.title}</h2>
        <span>{card.subTitle}</span>
      </div>
    {/each}
  </div>
  <i
    id="right"
    class="fa-solid fa-angle-right"
    on:click={btnScroll}
    on:keydown={btnScroll}
  />
</div>

<style>
  .wrapper {
    max-width: 60vw;
    width: 100%;
    position: relative;
  }

  .wrapper i {
    height: 2.5em;
    width: 2.5em;
    background: #fff;
    text-align: center;
    line-height: 2.5em;
    border-radius: 50%;
    cursor: pointer;
    position: absolute;
    top: 50%;
    font-size: 1.25rem;
    transform: translateY(-50%);
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.4);
  }

  .wrapper i:first-child {
    left: -22px;
  }

  .wrapper i:last-child {
    right: -22px;
  }
  .wrapper .carousel {
    display: grid;
    grid-auto-flow: column;
    grid-auto-columns: calc((100% / 3) - 9px);
    gap: 12px;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
    scroll-behavior: smooth;
    scrollbar-width: none;
  }

  .carousel::-webkit-scrollbar {
    display: none;
  }

  .carousel :where(.card, .img) {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .carousel .card {
    scroll-snap-align: start;
    height: 16em;
    padding-bottom: 0.2em;
    background: #fff;
    border-radius: 2.7em 0.7em;
    cursor: pointer;
  }

  .carousel.dragging {
    scroll-snap-type: none;
    scroll-behavior: auto;
  }

  .card .img {
    background: linear-gradient(
      to bottom right,
      var(--primary),
      50%,
      var(--secondary)
    );
    height: 8em;
    width: 10em;
    border-radius: 2.7em 0.7em;
    box-shadow: 2px 3px 6px rgba(0, 0, 0, 0.7);
  }

  .card .img img {
    width: 7em;
    height: 7em;
    border-radius: 50%;
    object-fit: cover;
    border: 0.3em solid #fff;
  }

  .card h2 {
    font-weight: 800;
    font-size: 1.3rem;
    margin: 0.6em 0 0.3em;
  }

  .card span {
    color: #4f5157;
    font-size: 1.1rem;
  }

  @media screen and (max-width: 950px) {
    .wrapper .carousel {
      grid-auto-columns: calc((100% / 2) - 7px);
    }
  }

  @media screen and (max-width: 650px) {
    .wrapper .carousel {
      grid-auto-columns: 100%;
    }
  }
</style>
