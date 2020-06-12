<script>
  import { loaderProgress } from "../stores";

  let progress = 0;

  const unsubscribeProject = loaderProgress.subscribe(value => {
    progress = value;
  });
</script>

<style>
  .tag-line {
    margin: 0;
    padding: 0;
    font-size: 1.35rem;
  }

  .project-name {
    margin: 0;
    padding: 0;
    font-size: 4rem;
  }

  .tag-desc {
    font-size: 2rem;
  }

  .text-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    max-height: 50%;
  }

  .loader-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .loader {
    width: 200px;
    opacity: 0.8;
    stroke: none;
    margin: -20px;
    padding: 0;
  }
</style>

<div class="home-container">
  <div class="text-container">
    <p class="tag-line">
      To serve and protect the great people of New York City
    </p>
    <h1 class="project-name">Project MURPHY</h1>
    <!-- <p class="tag-desc">Lorem ipsum dolor sit amet consectetur adipisicing elit. Tenetur perferendis ut dolorem
      consequatur porro sit corrupti mollitia aut fuga ipsa. Maxime possimus ullam esse dicta deserunt. Consequatur
      perspiciatis vitae officiis laboriosam maiores magni illum at commodi consequuntur quidem odit veniam placeat fuga
      vero suscipit, nisi voluptatibus, quod voluptates, aut assumenda quibusdam! Necessitatibus consequatur nemo
      similique ratione quas blanditiis molestiae adipisci consectetur tempore voluptas sint voluptate rem culpa iure
      itaque non laboriosam libero, repudiandae eaque! Culpa facere ea saepe officiis consectetur quis, veritatis quidem
      vitae sed totam sequi officia exercitationem vero facilis numquam nulla. Cumque nemo dolore voluptatum ratione
      doloribus autem.</p> -->
  </div>
  <div class="loader-container">
    {#if progress < 20}
      <h2>Loading..</h2>
      <svg
        class="loader"
        version="1.1"
        id="L6"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        x="0px"
        y="0px"
        viewBox="0 0 100 100"
        enable-background="new 0 0 100 100"
        xml:space="preserve">
        <rect
          fill="none"
          stroke="#fdd10a"
          stroke-width="4"
          x="25"
          y="25"
          width="50"
          height="50">
          <animateTransform
            attributeName="transform"
            dur="0.5s"
            from="0 50 50"
            to="180 50 50"
            type="rotate"
            id="strokeBox"
            attributeType="XML"
            begin="rectBox.end" />
        </rect>
        <rect x="27" y="27" fill="#fdd10a" width="46" height="50">
          <animate
            attributeName="height"
            dur="1.3s"
            attributeType="XML"
            from="50"
            to="0"
            id="rectBox"
            fill="freeze"
            begin="0s;strokeBox.end" />
        </rect>
      </svg>
      <h2>{progress * 5} %</h2>
    {/if}
  </div>
</div>
