## A perfect layout

> Webpage is structured in two sections with a animated carousel/slider.

![alt text](./capture.png)

Featuring:

Flex layout for single dimension structure like rows/columns, `nav` or `footer`.
Grid holds mostly `main` semantic thats multiple dimension. First lets flow the
`body` content into a flexing, vertical direction.

```css
body {
  margin: 0px;
  padding: 0px;
  /* Without this, an element with (h/w) means any padding & border, will
  increase the total space required by the element. Setting everything to
  `border-box` is recommended as it includes padding & border in now total
  element (h/w) needed in its position. Recommended global setting! */
  box-sizing: border-box;
  background-color: var(--background);

  height: 100vh;
  overflow: hidden;

  /* Flex 💪🏻 main/container in column for (navigation) and (main) */
  display: flex;
  flex-direction: column;
}
```

- Our `main` elements takes all remaining space leftover with `flex: 1`.
- Each `article` is finally the main grid container that translates in view.
- Ensure all sibling articles are absolutely positioned.

```css
/* Flex 💪🏻 sub-container configuration */
main {
  /* Ensure it takes all remaining vertical space by `body` Flex container */
  flex-grow: 1;
  position: relative;
  overflow: hidden; /* Prevent other article slides form 🤮 */
}

/* Grid 💪🏻 main/container configuration for each article */
main > article {
  display: grid;
  height: 100%;
  width: 100%;
  grid-template-columns: 2fr 1fr;
  grid-template-rows: 2fr 1fr;
}
/* Change `static` positioning on our article so we set them free 🕊️. */
/* Free 🕊️ them from default standard DOM flow constraints "in-flow". */
/* Remember! the body has an `overflow`, toggle this to see setup. */
main > article {
  position: absolute;
  left: 0px;
  top: 0px;
  transition: transform 400ms ease;
}

/* Our `data` attribute is used as selector with the following styles: */
main > article[data-status="inactive"] {
  transform: translateX(-100%);
  transition: none;
}
main > article[data-status="active"] {
  transform: translateX(0%);
  /* transition: none; */
}
main > article[data-status="before"] {
  transform: translateX(-100%);
  /* transition: none; */
}
main > article[data-status="becoming-active-from-left"] {
  transform: translateX(100%);
  transition: none;
}
main > article[data-status="after"] {
  transform: translateX(100%);
  /* transition: none; */
}
main > article[data-status="becoming-active-from-right"] {
  transform: translateX(-100%);
  transition: none;
}
```

- Manage your HTML with the `data` attribute.

```html
<!-- Step 1) Flex column (navigation) -->
<nav></nav>
<!-- Step 2) Flex column (main) ensure it takes remaining space -->
<main>
  <article data-index="0" data-status="active"></article>
  <article data-index="1" data-status="inactive"></article>
  <article data-index="2" data-status="inactive"></article>
  <article data-index="3" data-status="inactive"></div>
  </article>
</main>
```

The is slide the `articles` in a carousel fashion, out of visibility, when they
not active. Example, manage with `data` an articles index and current status. As
the page mounts, all articles matching this `data` selector are translated -100%
to the left, in the body overflow, hidden off the visible window.

- Below we manage the active and next slide with event callback functions.

```js
let activeIndex = 0;
const slides = document.getElementsByClassName("slide");

const handleDecrease = () => {
  // #1 - Decrease the `activeIndex` position, if condition:
  // If `activeIndex` is still greater than or equal to `>=` zero then we
  // can decrease our `nextIndex` by the active position. If we at the end
  // of the HTML collection, return to the last item in array with length.
  let nextIndex;
  if (activeIndex - 1 >= 0) nextIndex = activeIndex - 1;
  else nextIndex = slides.length - 1;

  // #2 - Query selectors `data-index` for all slides:
  // Here we get the active slide & next via `data` attribute:
  const activeSlide = document.querySelector(`[data-index="${activeIndex}"]`);
  const nextSlide = document.querySelector(`[data-index="${nextIndex}"]`);

  // #3 - Modify elements according to selector `data-status` styles:
  activeSlide.dataset.status = "after";
  nextSlide.dataset.status = "becoming-active-from-right";
  // Switch nextSlide to 'active' with timeout delay to apply style.
  setTimeout(() => {
    nextSlide.dataset.status = "active";
    // #4 - Here we keep the active index position to the next available.
    // Above we return index to zero if active position, reaches the end.
    // See css selector: `[data-status]` for more detail.
    activeIndex = nextIndex;
    // console.log(nextSlide.dataset.index);
  });
};
const handleIncrease = () => {
  // #1 - Increase the `activeIndex` position, if condition:
  // If `activeIndex` is still lesser or equal to `<=` array length then
  // we increase our `nextIndex` by the active position. Otherwise if we
  // greater than the HTML collection, return to the start of the array.
  let nextIndex;
  if (activeIndex + 1 <= slides.length - 1) nextIndex = activeIndex + 1;
  else nextIndex = 0;

  // #2 - Query selectors `data-index` for all slides:
  // Here we get the active slide & next via `data` attribute:
  const activeSlide = document.querySelector(`[data-index="${activeIndex}"]`);
  const nextSlide = document.querySelector(`[data-index="${nextIndex}"]`);
  // #3 - Modify elements according to selector `data-status` styles:
  activeSlide.dataset.status = "before";
  nextSlide.dataset.status = "becoming-active-from-left";
  // Switch nextSlide to 'active' with timeout delay to apply style.
  setTimeout(() => {
    nextSlide.dataset.status = "active";
    // #4 - Here we keep the active index position to the next available.
    // Above we return index to zero if active position, reaches the end.
    // See css selector: `[data-status]` for more detail.
    activeIndex = nextIndex;
    // console.log(nextSlide.dataset.index);
  });
};
```

Regards, <br />
Luigi Lupini <br />
<br />
I ❤️ all things (🇮🇹 / 🛵 / ☕️ / 👨‍👩‍👧)<br />
