<script>
    import cardsArray from "./+page.js"

    const imgsArr = Object.keys(import.meta.glob("$lib/images/**/*.*"));

let images = [
    { path: imgsArr[0], id: "img1" },
    { path: imgsArr[1], id: "img2" },
    { path: imgsArr[2], id: "img3" },
    { path: imgsArr[3], id: "img4" },
    { path: imgsArr[4], id: "img5" },
  ];
</script>



<div class="wrapper">
    <div class="carousel">
        {#each cardsArray as card}
{console.log(card)}
        {/each}
    </div>
</div>