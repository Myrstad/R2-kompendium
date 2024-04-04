<script>
  // REMEMBER: client:only directive when using
  function sanitizeId(text) {
    return text.replace(/\W+/g, '-').toLowerCase();
  }

  let data = []
  const sections = document.querySelectorAll("main section");
  sections.forEach(section => {
    const h2 = section.querySelector('h2')
    console.log(h2);
    const title = h2.innerText.slice(2)
    const id = sanitizeId(title)
    h2.setAttribute('id', id)
    const h3s = section.querySelectorAll('h3')
    let links = []
    h3s.forEach(h3=>{
      h3.setAttribute('id', sanitizeId(h3.innerText))
      links.push(["#"+sanitizeId(h3.innerText),h3.innerText])
    })
    let chapter = {
      id: id,
      title: title,
      links: links
    }

    data.push(chapter)
  })
  console.log(data);
  data.forEach(thing=>{
    console.log(thing);
  })
</script>

<nav>
  <div class="title">
    Navigasjon
  </div>
  {#each data as chapter}
    <div class="accordion">
      <input type="checkbox" id={"nav-"+chapter.id} class="input">
      <label for={"nav-"+chapter.id} class="label">{chapter.title}</label>
      <div class="content">
        <ul>
          {#each chapter.links as link}
            <li><a href={link[0]}>{link[1]}</a></li>
          {/each}
        </ul>
      </div>
    </div>
  {/each}
</nav>

<style>
  nav {
    /* position: sticky;
    top: 1rem; */
    /* max-height: calc(100dvh - 2rem); */
    background-color: var(--blue-100);
    border-radius: 3rem;
    font-family: var(--font-heading);
    padding-top: 2rem;
    padding-bottom: 1rem;
    padding-inline: 1rem;
    overflow-y: auto;
  }

  ul {
    margin: 0;
    padding: 0;
    display: grid;
    gap: .5rem;
  }

  li {
    list-style: none;
  }

  li a {
    background-color: var(--blue-200);
    color: black;
    font-weight: 600;
    font-size: 0.875rem;
    text-decoration: none;
    line-height: 114.285714286%;
    display: block;
    width: fit-content;
    padding: 0.5rem 0.5rem;
    border-radius: 0.5rem;
  }

  .title {
    font-size: 2rem;
    font-weight: 700;
    text-align: center;
  }

  .accordion {
    margin-top: 1rem;
    background-color: var(--blue-50);
    border-radius: 2rem;
  }
  
  .input {
    position: absolute;
    clip: rect(1px,1px,1px,1px);
    padding: 0;
    border: 0;
    height: 1px;
    width: 1px;
    overflow: hidden;
  }

  .label {
    border-radius: 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    cursor: pointer;
    padding-left: 2rem;
    padding-right: 0.5rem;
    height: 4rem;
    display: flex;
    align-items: center;
    font-size: 1.5rem;
    font-weight: 600;
  }

  .label::after {
    display: block;
    content: '\276F';
    width: 3rem;
    height: 3rem;
    border-radius: 1.5rem;
    display: grid;
    place-items: center;
    background-color: var(--blue-200);
    transform: rotate(90deg);
  }
  
  .content {
    padding-inline: 2rem;
    padding-block: 1rem;
    display: none;
  }

  .input:checked ~ .label {
    outline: 2px solid var(--blue-200);
    outline-offset: -1px;
  }

  .input:checked ~ .label::after {
    transform: rotate(-90deg);
  }

  .input:checked ~ .content {
    display: block;
  }
</style>