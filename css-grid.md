## CSS Grid

https://learncssgrid.com/

### Grid Template Areas

CSS for the template areas
```css
.wrapper {
  display: grid;
  grid-template-areas:
    'header header header'
    'nav article article';
  grid-template-rows: 60px 1fr;
  grid-template-columns: 20% 1fr 15%;
  grid-gap: 10px;
  height: 100vh;
  margin: 0;
}

header,
nav,
article {
  padding: 20px;
  background: lightgray;
}

#pageHeader {
  grid-area: header;
}
#mainArticle {
  grid-area: article;
}
#mainNav {
  grid-area: nav;
}
```

HTML for the template areas

```html
<div class="wrapper">
  <header id="pageHeader">
    Calles page
  </header>
  <article id="mainArticle">
    Content here
  </article>
  <nav id="mainNav">
    Navigation
  </nav>
</div>
```