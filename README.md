Isometric Map
----

This is an Isometric Map plugin for the [ImpactJS Javascript Game Engine](http://impactjs.com/) v1.19, released under the FreeBSD License.

Installation
----

### Copy files

Create the following directory:

`lib/plugins/isometricmap`

Place `isometric-map.js` and `isometric-calc.js` file into directory.

### Apply Assertion patch

Apply the patch `0002-[..].patch` to your installation of ImpactJS. This patch is designed for ImpactJS v1.19.

If you cannot (or do not want to) apply this patch, you must remove all of the calls to `ig.assert(...)`.

### Allow .loadLevel() loading

_Note that this step is entirely optional, but allows you to simply use the "this.loadLevel" style of loading a level._

#### Apply the patch
Apply the patch `0001-[..].patch` to your installation of ImpactJS. This patch is designed for ImpactJS v1.19

Usage
----

### Include the plugin

In your main.js, require the plugin:

```javascript
ig.module( 
	'game.main' 
)
.requires(
	'impact.game',
    'plugins.isometricmap.isometric-map',
)
.defines(function(){

    // ...

});
```

### Create a Map

Then create a new IsometricMap:

```javascript
init: function() {

    var data = [
        [1,2,6],
        [0,3,5],
        [2,8,1],
    ];
    var bg = new ig.IsometricMap( 16, data, 'media/tileset.png' );

},
```

### Alternatively, use Load Level

_Note: Requires the patch to be applied as stated above_

Requires manual editing of level files. Add a "type" attribute to each layer
with the class-name of IsometricMap.

Then your level file would look similar to:
(note the "type" attribute for the first "layer").

```javascript
LevelTest=/*JSON[*/{"entities":[],"layer":[{"type":"IsometricMap","name":"BG", ..., "data":[[1,4],[3,2]]}]}/*]JSON*/;
```

If no "type" specified, or an unknown class given, will default to "BackgroundMap".

This then allows you to do:

```javascript
init: function() {

    this.loadLevel(LevelTest);

},
```

Tiles
----

The isometric graphics used in game must follow the pattern:

```
      ##
    **  **
  **      **                ####                              ########
##          ## -OR-     ****    ****     -OR-         ********        ********         -OR- [...]
##          ##      ####            ####      ########                        ########
  **      **        ####            ####      ########                        ########
    **  **              ****    ****                  ********        ********
      ##                    ####                              ########
```

Where the edges marked (*) can be repeated to any length.

The number of horizontal pixels per vertical pixel must be a power of 2 greater
than 1. The example above correspond to 2, 4, and 8, respectively.

The graphic must be a square for each tile. Ie: if you have a tile which is 14px wide as in the first example above, then the graphic area it sits on must be 14px high (even though the tile itself is only 7px high).

Example:
![14x7 iso tile](http://i.imgur.com/YVjxX.png)

