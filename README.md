# Frontend Mentor - Meet landing page solution

This is a solution to the [Meet landing page challenge on Frontend Mentor](https://www.frontendmentor.io/challenges/meet-landing-page-rbTDS6OUR). Frontend Mentor challenges help you improve your coding skills by building realistic projects. 

## Table of contents

- [Overview](#overview)
  - [The challenge](#the-challenge)
  - [Screenshot](#screenshot)
  - [Links](#links)
- [My process](#my-process)
  - [Built with](#built-with)
  - [What I learned](#what-i-learned)
  - [Continued development](#continued-development)
  - [Useful resources](#useful-resources)
  - [AI Collaboration](#ai-collaboration)
- [Author](#author)

## Overview

### The challenge

Users should be able to:

- View the optimal layout depending on their device's screen size
- See hover states for interactive elements

### Screenshot

![](./screenshot.jpg)

### Links

- Solution URL: [Add solution URL here](https://your-solution-url.com)
- Live Site URL: [Add live site URL here](https://your-live-site-url.com)

## My process

### Built with

- Semantic HTML5 markup
- Flexbox
- CSS Grid
- Mobile-first workflow
- [Sass](https://sass-lang.com/) — SCSS syntax, 7-1 architecture pattern
  - Variables and `sass:color` for color management
  - Sass maps for breakpoints and color palettes
  - Mixins for responsive media queries
  - Placeholder selectors for shared style rules
  - Conditionals (`@if`) within mixins

### What I learned

The core learning of this project was working with Sass/SCSS at an architectural level, using the 7-1 pattern to distribute responsibilities across focused partials.

**Variables and the `sass:color` module**

Color management was centralized in a single partial. Beyond plain variable declarations, `sass:color` allowed generating derived values — like a semi-transparent color — programmatically:

```scss
@use 'sass:color';

$hsl-cyan-600: hsl(192, 37%, 48%);
$hsl-cyan-600-alpha: color.change($hsl-cyan-600, $alpha: 0.8);
```

**Sass maps for structured data**

Breakpoints and color palettes were stored as maps, making values accessible by name rather than by magic numbers:

```scss
$breakpoints: (
    'small':     23.4375em,
    'medium':    48em,
    'large':     62em,
    'xtralarge': 90em
);

$cyan-palete: (
    "primary":   $hsl-cyan-600,
    "secondary": $hsl-cyan-300,
    "tertiary":  $hsl-cyan-hover,
    "white":     $hsl-white
);
```

**Mixins for responsive design**

A single `respond-to` mixin wraps a `sass:map` lookup and `@content`, so breakpoints are referenced by name throughout the codebase:

```scss
@use 'sass:map';
@use './breakpoints';

@mixin respond-to($breakpoint) {
    @media (min-width: map.get(breakpoints.$breakpoints, $breakpoint)) {
      @content;
    }
}
```

Used in a layout partial to switch from a two-column to a four-column photo grid at the medium breakpoint:

```scss
&__hero {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: clamp(1em, 2.5vw, 2em);

    @include abstracts.respond-to('medium') {
        grid-template-columns: repeat(4, 1fr);
    }
}
```

**Placeholder selectors**

Shared typographic and layout rules were abstracted into placeholders to avoid duplication across partials:

```scss
%section-content-container {
    width: min(82.94%, 70em);
    margin-inline: auto;
}

%text-h2 {
    font-size: fonts.$font-size-h2;
    font-weight: 900;
}
```

Consumed via `@extend` wherever needed:

```scss
.site-first-section {
    @extend %section-content-container;

    &__title { @extend %text-h2; }
}
```

**Pseudo-elements `::before` / `::after` — section number connector line**

A decorative vertical line connecting the section number circles to the content above was rendered entirely with `::before` — no extra HTML element required. The combination of `position: relative` on the parent and `position: absolute` on the pseudo-element made precise placement straightforward:

```scss
%number-design {
    position: relative;

    &::before {
        display: block;
        content: '';
        height: 5em;
        width: .0625em;
        background-color: abstracts.$hsl-slate-300;
        position: absolute;
        top: 0;
        left: 50%;
        transform: translate(-50%, -100%);
    }
}
```

**`::before` / `::after` with CSS Grid — hero image columns**

On the desktop layout, the two hero images flanking the central content are rendered as pseudo-elements assigned to named grid areas — no additional HTML wrappers needed:

```scss
@include abstracts.respond-to('large') {
    display: grid;
    grid-template-columns: 1fr min(81.3333%, 28.01875em) 1fr;
    grid-template-areas: 'left center right';
    gap: 2em;

    &::before {
        content: '';
        background-image: url("../../starter-code/assets/desktop/image-hero-left.png");
        background-repeat: no-repeat;
        background-position: right clamp(0em, 3.5vw, 5.990625em) center;
        grid-area: left;
    }

    &::after {
        content: '';
        background-image: url("../../starter-code/assets/desktop/image-hero-right.png");
        background-repeat: no-repeat;
        background-position: left clamp(0em, 3.5vw, 5.990625em) center;
        grid-area: right;
    }
}
```

### Continued development

I want to keep using Sass/SCSS in future projects to deepen my understanding of its architecture and tooling. Specifically, I aim to:

- Explore more complex 7-1 pattern setups as projects grow in scope, refining how responsibilities are distributed across partials.
- Expand my use of Sass features — including functions, `@each` loops with maps, and the full module system (`@use` / `@forward`) — to write more maintainable and scalable stylesheets.
- Continue applying the patterns from this project (mixins for breakpoints, placeholder selectors for shared rules, color maps) as the foundation for my own style of authoring CSS.

### Useful resources

- [How to Install SASS Locally Using NPM](https://dev.to/adetutu/how-to-install-sass-locally-using-node-package-manager-npm-89) - An article on dev.to that explains how to install and use the SASS package locally via npm.
- [Sass Boilerplate](https://github.com/KittyGiraudel/sass-boilerplate.git) - GitHub repository documenting Sass's 7-1 architecture pattern, which served as the basis for the design of this solution.
- [CSS At-Rules](https://sass-lang.com/documentation/at-rules/css/) - An article from the Sass website that discusses media queries and other topics. It helped me understand how to use media queries in SCSS.
- [Breakpoints and media queries in SCSS](https://medium.com/codeartisan/breakpoints-and-media-queries-in-scss-46e8f551e2f2) - An article on Medium that discusses the use of breakpoints and media queries in mixins. It helped me define my media queries through a mixin.
- [Using responsive images in HTML](https://developer.mozilla.org/en-US/docs/Web/HTML/Guides/Responsive_images) - An MDN article discussing how to solve two problems related to the implementation of responsive images: art direction and resolution switching.
- [Posicionamiento absoluto](https://lenguajecss.com/css/posicionamiento/position-absolute/) - A section of the CSS page on manz.dev that discusses relative and absolute positioning. It helped me implement the design for the section numbers.
- [Mixins para media queries en Scss](https://css-tricks.com/snippets/sass/mixin-manage-breakpoints/) - An article from CSS-Tricks that demonstrates an approach to implementing media queries using SCSS mixins. It served as the basis for implementing the current mixin in this solution.
- [Atributo aria-hidden en acción](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-hidden) - An MDN article discussing the use of `aria-hidden`, used as a reference when deciding how to apply it to certain elements of this solution.
- [The difference between aria-label and aria-labelledby](https://tink.uk/the-difference-between-aria-label-and-aria-labelledby/) - An article on tink.uk that discusses the differences between the `aria-label` and `aria-labelledby` attributes, and how and when to use each one.
- [Sass Architecture](https://sass-guidelin.es/#architecture) - A section of the Sass Guidelines that covers the implementation of Sass architecture using the 7-1 pattern, its sections, and its partials.

### AI Collaboration

[Claude Code](https://claude.ai/code) (Anthropic) assisted throughout the development of this solution in the following areas:

**SCSS architecture and tooling**

Claude helped structure the SCSS partials following the 7-1 pattern — deciding which rules belonged in `abstracts/`, `base/`, `layout/`, and `components/`, and how partials should expose their members using `@use` and `@forward`. It provided guidance on when to prefer placeholder selectors over mixins, how to wire up the `respond-to` mixin with the breakpoints map, and how to leverage the `sass:color` and `sass:map` modules effectively. After each implementation step, Claude reviewed the code and offered post-implementation analysis on whether the approach was semantically appropriate and consistent with Sass best practices.

**HTML semantics and accessibility**

Claude reviewed `index.html` and suggested improvements to the document's semantic structure and accessibility attributes — including correct use of `aria-labelledby` to associate section headings with their landmark regions, `aria-label` on buttons that lacked descriptive visible text, and `aria-hidden` on purely decorative elements. These changes aligned the document with ARIA best practices without affecting the visual design.

**README**

Claude helped draft this README, structuring the "What I learned" section around the most meaningful SCSS concepts from the project and selecting code excerpts that best illustrated those learnings.

## Author

- **Name** - Víctor Suquilanda
- **Frontend Mentor** - [@victor-sc12](https://www.frontendmentor.io/profile/victor-sc12)
- **GitHub** - [@victor-sc12](https://github.com/victor-sc12)
