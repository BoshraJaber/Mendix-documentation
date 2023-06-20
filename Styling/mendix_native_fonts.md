# Mendix Native Fonts
## Everything I learnt working with Mendix Native fonts:

1. **Setting Up fonts:**
   - Unlike web fonts, native fonts has to be added during the build of the application. Fonts classes will be provided by Mendix Builder
   - This [documentation](https://docs.mendix.com/howto8/mobile/native-custom-fonts/) gives detailed steps on how to do so.

2. **Responsive fonts:**
   - Having the same fonts for all devices might not be a good option. Mendix already offers a build-in helper function to adjust the fonts based on the width and height of the device used. This function is called `adjustFont`, usually found in the `theme\styles\native\core\helpers\_functions\adjustfont.js` folder.
. One option would be calling this function whenever using `fontSize` property.

    - What if this function doesn't fit your needs? the following two solutions are possible:
        1. Using the build-in method from React Native.
        2. Building your own custom function, this method I recomment the most, it gives you more flexibilty and control. the idea is to choose a base device where the fonts will look as they are. 
Then the fonts will scale down for smaller devices, and up for larger ones. An Example of that:
```js
import { PixelRatio } from "react-native";

export function adjustFontsReactNativeWay(size) {
  return size / PixelRatio.getFontScale()
}
```
or
```js
export function adjustFontsCustom(size) {
  // Use iPhone11 as base size which is 414 x 896
// base device recommended to be the one used in figma
  const baseWidth = 414;
  const baseHeight = 896;
  const scaleWidth = width / baseWidth;
  const scaleHeight = height / baseHeight;
  const scale = Math.min(scaleWidth, scaleHeight);
  return Math.ceil((size * scale));
}
```
![](https://raw.githubusercontent.com/heyman333/react-native-responsive-fontsize/HEAD/images/main.png)


3. **Common issues with fonts:**
- Classes genertated by Mendix is not usable, the way to solve it is to choose differnt fonts file. The classes might look like this:
```js
export const nexaBoldFontFamily = {
    ☞: "Nexa-Bold",
};
```
- The fonts are effected by the fonts of the device using the app, this could cause a lot of UIs issue if not handdles. However, this can be turened off by adding these couple of lines:
```js
import { Text } from "react-native";

Text.defaultProps = Text.defaultProps || {};
Text.defaultProps.allowFontScaling = false;
```

4. Useful Resources:
- [Mobile viewports for responsive experiences](https://experienceleague.adobe.com/docs/target/using/experiences/vec/mobile-viewports.html)
- [PixelRatio - React Native](https://reactnative.dev/docs/pixelratio)

