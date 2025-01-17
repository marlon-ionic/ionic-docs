---
title: 'Infinite Scroller | ion-infinite-scroll Action Component'
description: 'The ion-infinite-scroll component calls an action to be performed when the user scrolls a specified distance from the bottom or top of the page.'
sidebar_label: 'ion-infinite-scroll'
demoUrl: '/docs/demos/api/infinite-scroll/index.html'
demoSourceUrl: 'https://github.com/ionic-team/ionic-docs/tree/main/static/demos/api/infinite-scroll/index.html'
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# ion-infinite-scroll

The Infinite Scroll component calls an action to be performed when the user scrolls a specified distance from the bottom or top of the page.

The expression assigned to the `ionInfinite` event is called when the user reaches that defined distance. When this expression has finished any and all tasks, it should call the `complete()` method on the infinite scroll instance.

## Infinite Scroll Content

The `ion-infinite-scroll` component has the infinite scroll logic. It requires a child component in order to display content. Ionic uses its `ion-infinite-scroll-content` component by default. This component displays the infinite scroll and changes the look depending on the infinite scroll's state. It displays a spinner that looks best based on the platform the user is on. However, the default spinner can be changed and text can be added by setting properties on the `ion-infinite-scroll-content` component.

## Custom Content

Separating the `ion-infinite-scroll` and `ion-infinite-scroll-content` components allows developers to create their own content components, if desired. This content can contain anything, from an SVG element to elements with unique CSS animations.

## Usage

<Tabs defaultValue="angular" values={[{ value: 'angular', label: 'ANGULAR' }, { value: 'javascript', label: 'JAVASCRIPT' }, { value: 'stencil', label: 'STENCIL' }, { value: 'vue', label: 'VUE' }]}>

<TabItem value="angular">

```html
<ion-content>
  <ion-button (click)="toggleInfiniteScroll()" expand="block">
    Toggle Infinite Scroll
  </ion-button>

  <ion-list></ion-list>

  <ion-infinite-scroll threshold="100px" (ionInfinite)="loadData($event)">
    <ion-infinite-scroll-content
      loadingSpinner="bubbles"
      loadingText="Loading more data..."
    >
    </ion-infinite-scroll-content>
  </ion-infinite-scroll>
</ion-content>
```

```tsx
import { Component, ViewChild } from '@angular/core';
import { IonInfiniteScroll } from '@ionic/angular';

@Component({
  selector: 'infinite-scroll-example',
  templateUrl: 'infinite-scroll-example.html',
  styleUrls: ['./infinite-scroll-example.css'],
})
export class InfiniteScrollExample {
  @ViewChild(IonInfiniteScroll) infiniteScroll: IonInfiniteScroll;

  constructor() {}

  loadData(event) {
    setTimeout(() => {
      console.log('Done');
      event.target.complete();

      // App logic to determine if all data is loaded
      // and disable the infinite scroll
      if (data.length == 1000) {
        event.target.disabled = true;
      }
    }, 500);
  }

  toggleInfiniteScroll() {
    this.infiniteScroll.disabled = !this.infiniteScroll.disabled;
  }
}
```

</TabItem>

<TabItem value="javascript">

```html
<ion-content>
  <ion-button onClick="toggleInfiniteScroll()" expand="block">
    Toggle Infinite Scroll
  </ion-button>

  <ion-list></ion-list>

  <ion-infinite-scroll threshold="100px" id="infinite-scroll">
    <ion-infinite-scroll-content
      loading-spinner="bubbles"
      loading-text="Loading more data..."
    >
    </ion-infinite-scroll-content>
  </ion-infinite-scroll>
</ion-content>
```

```javascript
const infiniteScroll = document.getElementById('infinite-scroll');

infiniteScroll.addEventListener('ionInfinite', function (event) {
  setTimeout(function () {
    console.log('Done');
    event.target.complete();

    // App logic to determine if all data is loaded
    // and disable the infinite scroll
    if (data.length == 1000) {
      event.target.disabled = true;
    }
  }, 500);
});

function toggleInfiniteScroll() {
  infiniteScroll.disabled = !infiniteScroll.disabled;
}
```

</TabItem>

<TabItem value="stencil">

```tsx
import { Component, State, h } from '@stencil/core';

@Component({
  tag: 'infinite-scroll-example',
  styleUrl: 'infinite-scroll-example.css',
})
export class InfiniteScrollExample {
  private infiniteScroll: HTMLIonInfiniteScrollElement;

  @State() data = [];

  componentWillLoad() {
    this.pushData();
  }

  pushData() {
    const max = this.data.length + 20;
    const min = max - 20;

    for (var i = min; i < max; i++) {
      this.data.push('Item ' + i);
    }

    // Stencil does not re-render when pushing to an array
    // so create a new copy of the array
    // https://stenciljs.com/docs/reactive-data#handling-arrays-and-objects
    this.data = [...this.data];
  }

  loadData(ev) {
    setTimeout(() => {
      this.pushData();
      console.log('Loaded data');
      ev.target.complete();

      // App logic to determine if all data is loaded
      // and disable the infinite scroll
      if (this.data.length == 1000) {
        ev.target.disabled = true;
      }
    }, 500);
  }

  toggleInfiniteScroll() {
    this.infiniteScroll.disabled = !this.infiniteScroll.disabled;
  }

  render() {
    return [
      <ion-content>
        <ion-button onClick={() => this.toggleInfiniteScroll()} expand="block">
          Toggle Infinite Scroll
        </ion-button>

        <ion-list>
          {this.data.map(item => (
            <ion-item>
              <ion-label>{item}</ion-label>
            </ion-item>
          ))}
        </ion-list>

        <ion-infinite-scroll
          ref={el => (this.infiniteScroll = el)}
          onIonInfinite={ev => this.loadData(ev)}
        >
          <ion-infinite-scroll-content
            loadingSpinner="bubbles"
            loadingText="Loading more data..."
          ></ion-infinite-scroll-content>
        </ion-infinite-scroll>
      </ion-content>,
    ];
  }
}
```

</TabItem>

<TabItem value="vue">

```html
<template>
  <ion-page>
    <ion-content class="ion-padding">
      <ion-button @click="toggleInfiniteScroll" expand="block">
        Toggle Infinite Scroll
      </ion-button>

      <ion-list>
        <ion-item v-for="item in items" :key="item">
          <ion-label>{{ item }}</ion-label>
        </ion-item>
      </ion-list>

      <ion-infinite-scroll
        @ionInfinite="loadData($event)"
        threshold="100px"
        id="infinite-scroll"
        :disabled="isDisabled"
      >
        <ion-infinite-scroll-content
          loading-spinner="bubbles"
          loading-text="Loading more data..."
        >
        </ion-infinite-scroll-content>
      </ion-infinite-scroll>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
  import {
    IonButton,
    IonContent,
    IonInfiniteScroll,
    IonInfiniteScrollContent,
    IonItem,
    IonLabel,
    IonList,
    IonPage,
  } from '@ionic/vue';
  import { defineComponent, ref } from 'vue';

  export default defineComponent({
    components: {
      IonButton,
      IonContent,
      IonInfiniteScroll,
      IonInfiniteScrollContent,
      IonItem,
      IonLabel,
      IonList,
      IonPage,
    },
    setup() {
      const isDisabled = ref(false);
      const toggleInfiniteScroll = () => {
        isDisabled.value = !isDisabled.value;
      };
      const items = ref([]);
      const pushData = () => {
        const max = items.value.length + 20;
        const min = max - 20;
        for (let i = min; i < max; i++) {
          items.value.push(i);
        }
      };

      const loadData = (ev: CustomEvent) => {
        setTimeout(() => {
          pushData();
          console.log('Loaded data');
          ev.target.complete();

          // App logic to determine if all data is loaded
          // and disable the infinite scroll
          if (items.value.length == 1000) {
            ev.target.disabled = true;
          }
        }, 500);
      };

      pushData();

      return {
        isDisabled,
        toggleInfiniteScroll,
        loadData,
        items,
      };
    },
  });
</script>
```

</TabItem>

</Tabs>

## Properties

### disabled

|                 |                                                                                                                                                                                                                                                                                                                                                               |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Description** | If `true`, the infinite scroll will be hidden and scroll event listeners<br />will be removed.<br /><br />Set this to true to disable the infinite scroll from actively<br />trying to receive new data while scrolling. This is useful<br />when it is known that there is no more data that can be added, and<br />the infinite scroll is no longer needed. |
| **Attribute**   | `disabled`                                                                                                                                                                                                                                                                                                                                                    |
| **Type**        | `boolean`                                                                                                                                                                                                                                                                                                                                                     |
| **Default**     | `false`                                                                                                                                                                                                                                                                                                                                                       |

### position

|                 |                                                                                              |
| --------------- | -------------------------------------------------------------------------------------------- |
| **Description** | The position of the infinite scroll element.<br />The value can be either `top` or `bottom`. |
| **Attribute**   | `position`                                                                                   |
| **Type**        | `"bottom" \| "top"`                                                                          |
| **Default**     | `'bottom'`                                                                                   |

### threshold

|                 |                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Description** | The threshold distance from the bottom<br />of the content to call the `infinite` output event when scrolled.<br />The threshold value can be either a percent, or<br />in pixels. For example, use the value of `10%` for the `infinite`<br />output event to get called when the user has scrolled 10%<br />from the bottom of the page. Use the value `100px` when the<br />scroll is within 100 pixels from the bottom of the page. |
| **Attribute**   | `threshold`                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **Type**        | `string`                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **Default**     | `'15%'`                                                                                                                                                                                                                                                                                                                                                                                                                                 |

## Events

| Name          | Description                     |
| ------------- | ------------------------------- |
| `ionInfinite` | Emitted when the scroll reaches |

the threshold distance. From within your infinite handler,
you must call the infinite scroll's `complete()` method when
your async operation has completed. |

## Methods

### complete

|                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Description** | Call `complete()` within the `ionInfinite` output event handler when<br />your async operation has completed. For example, the `loading`<br />state is while the app is performing an asynchronous operation,<br />such as receiving more data from an AJAX request to add more items<br />to a data list. Once the data has been received and UI updated, you<br />then call this method to signify that the loading has completed.<br />This method will change the infinite scroll's state from `loading`<br />to `enabled`. |
| **Signature**   | `complete() => Promise<void>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
