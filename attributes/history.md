
### `history` attribute for `<game-scene>`

#### Description:

The `history` attribute of the `<game-scene>` element serves as a record of the user's navigation through different game levels within the same game scene. It is automatically updated based on the user's movement between and within levels.

#### Type:

- String: It is a comma-separated list of `levelPath` entries, where each `levelPath` represents a user's journey within or between game levels.

#### Specification:

1. **Updating `history`**:
    - Every time the user navigates to a new game level or within the same game level, the `history` attribute should be updated to append the new `levelPath` at the end.
    - The `levelPath` is a combination of the `id` of the `<game-level>` element and the `id` of a child element inside the `<game-level>`, separated by a `/`.
    - Example: If a user navigates to a game level with the `id` "level1" and reaches a stage with the `id` "stage_boss", the `levelPath` will be `level1/stage_boss`.

2. **Navigating Back and Updating `history`**:
    - If the user goes back in history (for instance, by an in-game back action or some other mechanic) and then chooses a new path or level, the `history` attribute must adapt to reflect this new state.
    - All `levelPath` entries that exist after the previous level (i.e., the level before the user took the new path) should be deleted.
    - The new `levelPath` should then be appended to the history.

3. **Example Scenario**:
    - Given a history like: `level1/stage1,level2/stage2,level3/stage_boss`
    - If the user goes back from `level3` to `level2` and then chooses a different path within `level2`, say `stage3`, the new history should look like: `level1/stage1,level2/stage3`

#### Usage:

Suppose a game scene with three game levels:

```html
// default view on page load
<game-scene history="level1/stage1">
    <game-level id="level1">...</game-level>
</game-scene>

// then user clicks to get to the level 2
// game scene is updated consequently
<game-scene history="level1/stage1, level2/stage1">
    <game-level id="level2">...</game-level>
</game-scene>

// then the user goes back to level1/stage1
// and chooses to go to level 3
<game-scene history="level1/stage1, level3/stage1">
    <game-level id="level2">...</game-level>
</game-scene>
```


If the user navigates through the levels and stages in the following sequence:

1. `level1/stage1`
2. `level1/stage2`
3. `level2/stage1`
4. Goes back to `level1/stage2`
5. Chooses a different path: `level1/stage3`

The final `history` attribute should look like:

```
history="level1/stage1,level1/stage3"
```

#### Interaction with JavaScript:

Developers can leverage JavaScript event listeners to monitor changes to the `history` attribute, allowing for game mechanics that depend on the player's navigation history.

```javascript
let gameScene = document.querySelector('game-scene');
gameScene.addEventListener('attributeChanged', (event) => {
    if (event.attributeName === 'history') {
        console.log('History changed:', gameScene.getAttribute('history'));
    }
});
```

---

This specification ensures the `history` attribute provides a reliable record of user navigation, adapting dynamically to changes in user choices. It provides a basis for more advanced in-game mechanisms like branching narratives or achievements based on paths chosen.