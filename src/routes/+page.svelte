<script>
  // https://www.youtube.com/watch?v=6QE8dXq9SOE&t=1s

  // npm install --save @fortawesome/fontawesome-free

  import "../app.css";

  let carousel;

  let isDragging = false;

  const dragStart = () => {
    isDragging = true;
    document.querySelector(".carousel").classList.add("dragging");
  };

  const dragStop = () => {
    isDragging = false;
    document.querySelector(".carousel").classList.remove("dragging");
  };

  const dragging = e => {
    if (!isDragging) return;
    carousel.scrollLeft = e.pageX;
  };

  const imgsArr = Object.keys(import.meta.glob("$lib/images/**/*.*"));

  let images = [
    { path: imgsArr[0], id: "img1" },
    { path: imgsArr[1], id: "img2" },
    { path: imgsArr[2], id: "img3" },
    { path: imgsArr[3], id: "img4" },
    { path: imgsArr[4], id: "img5" },
  ];

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

<div class="wrapper">
  <i class="fa-solid fa-angle-left" />
  <div
    class="carousel"
    bind:this={carousel}
    on:mousemove={dragging}
    on:mousedown={dragStart}
  >
    {#each cardsArray as card}
      <div class="card">
        <div class="img" draggable="false"
          ><img src={card.image} alt={card.title} /></div
        >
        <h2>{card.title}</h2>
        <span>{card.subTitle}</span>
      </div>
    {/each}
  </div>
  <i class="fa-solid fa-angle-right" />
</div>

<style>
  .wrapper {
    max-width: 1100px;
    width: 100%;
    position: relative;
  }

  .wrapper i {
    height: 50px;
    width: 50px;
    background: #fff;
    text-align: center;
    line-height: 50px;
    border-radius: 50%;
    cursor: pointer;
    position: absolute;
    top: 50%;
    font-size: 1.25rem;
    transform: translateY(-50%);
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.23);
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
    grid-auto-columns: calc((100% / 3) - 12px);
    gap: 16px;
    overflow: hidden;
  }

  .carousel :where(.card, .img) {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .carousel .dragging .card {
    cursor: grab;
    user-select: none;
  }
  .carousel .card {
    height: 342px;
    padding-bottom: 15px;
    background: #fff;
    border-radius: 2.7em 0.7em;
    cursor: pointer;
  }

  .card .img {
    background: #8b53ff;
    height: 148px;
    width: 148px;
    border-radius: 2.7em 0.7em;
  }

  .card .img img {
    width: 140px;
    height: 140px;
    border-radius: 50%;
    object-fit: cover;
    border: 4px solid #fff;
  }

  .card h2 {
    font-weight: 800;
    font-size: 1.56rem;
    margin: 30px 0 5px;
  }

  .card span {
    color: #6a6d78;
    font-size: 1.31rem;
  }

  @media screen and (max-width: 900px) {
    .wrapper .carousel {
      grid-auto-columns: calc((100% / 2) - 9px);
    }
  }

  @media screen and (max-width: 600px) {
    .wrapper .carousel {
      grid-auto-columns: 100%;
    }
  }
</style>
