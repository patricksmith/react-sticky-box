# React Sticky Box

The code is pretty much done. Now I just need to find some time to document everything. So stay tuned!

## Idea

We know lots of pages with sticky navigations. But what happens if the navigation takes up more height than the viewport? StickyBox solves this issue by allowing to scroll sticky positioned elements if they are too big.

## Usage

```jsx
<SomeContainer>
  <StickyBox width={200}>
    <div className="sidebar">...sidebar...</div>
  </StickyBox>
  <div className="main">...lots of content...</div>
</SomeContainer>
```

## Changelog

### 0.4 -> 0.5

completely rewritten the engine - using position fixed and position absolute. This will lead to almost no jank.

This change causes some new limitations:

- you need to explicitely state the width of the StickyBox or use `width="measure"` to measure its child for its dimensions. (e.g. `<StickyBox width={200}><b>content</b></StickyBox>` or `<StickyBox width="measure"><div style={{width: myWidth}}>content</div></StickyBox>`)
- no more support for margins on the `<StickyBox/>`. I.e. `<StickyBox style={{marginTop: 20}}/>` etc. is not supported and will lead to undefined behaviour. Use paddings or margins on the parent and/or child nodes instead. (e.g. `<div style={{padding: 20}}><StickyBox width={100}><b style={{margin: 10}}>content</b></StickyBox></div>`)
- by default the old `stickToTop` behaviour is active, `stickToBottom` is now simply `bottom`

Background: browser started more and more to not reliably fire the `onScroll` and `onMouseWheel` event since their engines started moving scrolling to some async thread for performance reasons. This meant that a lot of scrolling could happen without react-sticky-box getting notified.

By using `position:fixed` and switching to `position:absolute` when certain scroll boundaries are reached, there's no more jank while scrolling. In the worst case a jump will happen when reaching one of these boundaries. But that's much better than jumping almost every frame when scrolling!
