# Loadmore

> A two-direction pull-to-refresh component. Custom HTML templates supported.

-------------

## Import

```javascript
import { Loadmore } from 'mint-ui';

Vue.component(Loadmore.name, Loadmore);
```

## Example

```html
<mt-loadmore :top-method="loadTop" :bottom-method="loadBottom" :bottom-all-loaded="allLoaded">
  <ul>
    <li v-for="item in list">{{ item }}</li>
  </ul>
</mt-loadmore>
```

Take upward direction for example: pull the component `topDistance` pixels away from the top and then release it, the function you appointed as `top-method` will run

```javascript
loadTop(id) {
  ...// load more data
  this.$broadcast('onTopLoaded', id);
}
```
At the end of your `top-method`, don't forget to broadcast the `onTopLoaded` event so that `mint-loadmore` removes `topLoadingText`. Note that a parameter called `id` is passed to `loadTop` and `onTopLoaded`. This is because after the top data is loaded, some reposition work is performed inside a `mint-loadmore` instance, and `id` simply tells the component which instance should be repositioned. You don't need to do anything more than passing `id` to `onTopLoaded` just as shown above.

For downward direction, things are similar. To invoke `bottom-method`, just pull the component `bottomDistance` pixels away from the bottom and then release it

```javascript
loadBottom(id) {
  ...// load more data
  this.allLoaded = true;// if all data are loaded
  this.$broadcast('onBottomLoaded', id);
}
```
The only difference is that after all data are fetched, you can set `bottom-all-loaded` to `true` so that `bottom-method` will not run any more.

## Custom HTML templates

You can customize the top and bottom DOM using an HTML template
```html
<mt-loadmore :top-method="loadTop" :top-status.sync="topStatus">
  <ul>
    <li v-for="item in list">{{ item }}</li>
  </ul>
  <div slot="top" class="mint-loadmore-top">
    <span v-show="topStatus !== 'loading'" :class="{ 'rotate': topStatus === 'drop' }">↓</span>
    <span v-show="topStatus === 'loading'">Loading...</span>
  </div>
</mt-loadmore>
```
For example, to customize the top DOM, you'll need to add a variable that syncs with `top-status` on `loadmore` tag, and then write your template with a `slot` attribute set to `top` and `class` set to `mint-loadmore-top`. `top-status` has three possible values that indicates which status the component is at
*  `pull`: if the component is being pulled yet not ready to drop (top distance is within the distance threshold defined by `topDistance`)
*  `drop`: if the component is ready to drop
*  `loading`: if `topMethod` is running

## Configure texts in top and bottom DOM
If you decide not to customize HTML templates, you can configure the texts that comes with `loadmore`. Take the top DOM for example, corresponding to the three `top-status` states, configurable options are: `topPullText`, `topDropText` and `topLoadingText`. And `bottomPullText`, `bottomDropText` and `bottomLoadingText` are for the bottom DOM.

## API
| option | description | type | acceptable values | default |
|------|-------|---------|-------|--------|
| topPullText | top text when the component is being pulled down | String | | '下拉刷新' |
| topDropText | top text when the component is ready to drop | String | | '释放更新' |
| topLoadingText | top text while `topMethod` is running | String | | '加载中...' |
| topDistance | distance threshold that triggers `topMethod`(in pixel) | Number | | 70 |
| topMethod | upward load-more function | Function | | |
| bottomPullText | bottom text when the component is being pulled up | String | | '上拉刷新' |
| bottomDropText | bottom text when the component is ready to drop | String | | '释放更新' |
| bottomLoadingText | bottom text while `bottomMethod` is running | String | | '加载中...' |
| bottomDistance | distance threshold that triggers `bottomMethod`(in pixel) | Number | | 70 |
| bottomMethod | downward load-more function | Function | | |
| bottomAllLoaded | if `true`, `bottomMethod` can no longer be triggered | Boolean | | false |

## Slot
| name | description |
|------|--------|
| - | data list |
| top | custom top HTML template |
| bottom | custom bottom HTML template |