# Style Stack

Style Stack is a configurable style library built with ShadowDOM in mind.

The methodology within Style Stack's css is not unlike other css libraries in existence and, as we all know, there's no shortage of those. However, there are a few aspects of Style Stack that I believe makes it valuable.

Firstly, Style Stack strictly subscribes to the use of relative units `rem`, `em` and `%` to easily provide adaptive behavior. A percentage-based html font-size combined with configurable media query "scales" allow the implementor to easily adjust the size of the entire layout. Because components also utilize relative values, they too will scale. This provides automatic adaptations at various screen sizes without requiring a litany of media-specific styles and classes. This, combined with optional, query-specific modifier classes provides an easy-to-implement yet very flexible layout system.

Secondly, the component mixins are isolated in such a way that they do not generate required nested selectors or selector chains. The primary motivation of this is usage with WebComponent / ShadowDOM selectors such as `:host`, `::slotted()` and `[theme='blue']`.

> An added benefit of this approach is that your standard DOM elements can remain stylistically identical to your ShadowDOM elements without managing duplicate stylesheets.

Trivial Example:

```scss
// container.scss

@import 'style-stack/lib/components/container.lib';

:host {
    @include container();
}

:host([max-width='true']) {
    @include container-max-width();
}
```

```script
// container.js

class Container extends LitElement {
    static get properties() {
        return {
            'max-width' : {
                type: Boolean,
                reflect: true
            }
        }
    }

    render() {
        return html`
            <slot></slot>
        `
    }
}

customElements.define('my-container', Container);
```

# Overview

Style Stack is organized into 3 feature sets:
- Base
- Components
- Modifiers

*Base* styles are universally applied styles and defaults. This includes fonts, generic pseudo styles such as `::selection` and preferred defaults such as `box-sizing`.

*Components* are characterized by the fact that they are applied to a host and typically require specific markup structure when composed of multiple elements. For example, the `group` component is used to create layouts like so:

```html
<section class="group group-justify-center">
    <div class="group-item group-item-size-16">
        <h1>Hello, World!</h1>
    </div>
</section>
```

*Modifiers* are generic utilities that can be applied to any component or element to modify properties such as color and text alignment. For example, any configured theme color can be applied as a background using `color` modifiers:

```html
<div class="container color-blue-bg">
    <h1>Hello, World!</h1>
</div>
```

