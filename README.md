# The Name Button (The Stanley Parable: Ultra Deluxe)

A lightweight, self-contained, CSS-animated SVG replica of "The button that says the name of the player that is playing the game™" from *The Stanley Parable: Ultra Deluxe*. 

This project goes beyond a simple vector graphic. It is a fully interactive, zero-dependency web component containing its own neo-brutalist CSS physics, keyboard accessibility, and a bundled audio payload, all packed into a single `.svg` file.

## Features

- **Self-Contained Audio:** The soundbite is encoded directly inside the SVG as a 4kbps OPUS Data URI. The entire audio payload is roughly 2KB, resulting in zero external network requests and instantaneous playback.
- **Flat 3D Physics:** The SVG separates the button's "surface" from its "thickness." CSS transforms are used to physically compress the top layer into the stationary base, creating a highly satisfying, tactile press effect.
- **Rapid-fire Ready:** The internal audio object resets instantly on click, allowing for relentless button mashing without audio overlap or delay.
- **Accessible:** Includes tab-indexing, focus management, and keyboard event listeners (`Space` and `Enter`) mapped to both the visual CSS state and the audio trigger.

## Usage

Because this SVG contains embedded JavaScript and audio, **you cannot import it using a standard `<img>` tag**. Browsers strictly block scripts inside `<img>` tags for security reasons. 

To retain interactivity, you must embed the file using an `<object>` tag:

```html
<object data="the-name-button.svg" type="image/svg+xml" class="the-name-button" aria-label="The Name Button" tabindex="0">
  Your browser does not support SVG objects.
</object>
```

## Technical Gotchas & Implementation Notes

If you plan on modifying the `.svg` file or implementing it in a complex environment, keep these architectural quirks in mind:

### XML Parsing of Scripts

SVGs are XML documents. If you write standard JavaScript inside an SVG `<script>` tag, characters like `<` (less than), `>` (greater than), or `&` (ampersand) will cause the XML parser to fail, breaking the entire image. 

To fix this, the JavaScript inside `the-name-button.svg` is wrapped in a CDATA section:
```javascript
<script>
// <![CDATA[
  ... all javascript logic here ...
// ]]>
</script>
```
This tells the XML parser to treat everything inside the block as raw character data, keeping the script intact.

### Focus Ring

By default, browsers apply a blue outline to focused interactive elements. Because we use CSS `filter: drop-shadow()` to create the shadow of the button housing, a frustrating bug occurs: the drop-shadow filter photocopies *everything* rendered on the element, including the browser's default blue focus ring. 

To prevent the button from generating a massive blue shadow when clicked, `outline: none;` is explicitly declared inside the SVG's internal CSS. If you add your own wrapper styles, ensure you don't accidentally re-enable default outlines on the `<object>` tag itself.

### Audio Autoplay

Even though the audio is a local Data URI, modern browser autoplay policies still apply. The `AudioContext` requires a user gesture (a direct click, tap, or keydown event) before it will allow sound to play. The script handles this perfectly for human interaction, but be aware that you cannot programmatically trigger the button's sound via external scripts without the user interacting with the page first.

### Mobile Tap Delays & Highlights

To make the button feel responsive on touch screens, the internal CSS includes `-webkit-tap-highlight-color: transparent;` to hide the default grey/blue tap flash on iOS and Android. Additionally, the script listens for `touchstart` events and calls `e.preventDefault()` to bypass the standard 300ms click delay imposed by mobile browsers.

## Contributing

Pull requests are welcome. If you find a way to make the button say a name other than the name of the player that is playing the game, please immediately report yourself to the facility manager.

## Links

- [The Stanley Parable Website](https://www.stanleyparable.com/)
- [The Name Button Wiki](https://thestanleyparable.fandom.com/wiki/The_button_that_says_the_name_of_the_player_that_is_playing_the_game)
- [My Steam Profile Completion :P](https://steamcommunity.com/id/varungupta3009/stats/1703340)
- [My Commitment to "Commitment"](https://www.reddit.com/r/stanleyparable/comments/urtejs/i_acheived_both_the_commitment_achievements/)

## License

[MIT License](LICENSE)