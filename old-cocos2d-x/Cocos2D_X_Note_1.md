> Coco2D-X Note 1

# Concepts

## Scene
- `renderer` to show the scene
### Scene Graph (Scene Tree)
- on tree : `object nodes`
- using InOrder traversal to get the priority
- don't mind, use the function next to pile object nodes
```cpp
scene -> addChild(some_objnode, z-order);
```
*where `z-order` is an int showing the priority the obj to be showed, the bigger, the fronter*

## Sprite
```cpp
auto mySprite = Sprite::create("mysprite.png");
//changing the properties below
mySprite -> setPosition(Vec2(500, 0));
mySprite -> setRotation(40);
mySprite -> setScale(2.0); // sets both X and Y uniformly
mySprite -> set AnchorPoint(Vec2(0, 0)); // referenced base point on this sprite when calculating its coordinates, def in [0, 1] as proportion
```

## Action
```cpp
auto mySprite = Sprite::create("mysprite.png");
// move 50px right, 10px up after 2 sec
auto moveBy = MoveBy::create(2, Vec2(50, 10));
mySprite -> runAction(moveBy);
```

## Sequence & Spawn
- multiple actions together, order or rev-order
- the code below : $MoveTo\to delay\to MoveBy\to delay \to MoveTo$
```cpp
auto moveTo1 = MoveTo::create(2, Vec2(50, 10));
auto moveBy1 = MoveBy::create(2, Vec2(100, 10));
auto moveTo2 = MoveTo::create(2, Vec2(150, 10));
auto delay = DelayTime::create(1);
//Actions one by one
mySprite -> runAction(Sequence::create(moveTo1, delay, moveBy1, delay.clone(), moveTo2, nullptr));
//Actions together at one time : use Spawn
mySprite -> runAction(Spawn::create(moveTo1, moveBy1, moveTo2, nullptr));
```

## Relationship Between Nodes
- Father and Son
- more like a bond (I think)

## Logs
- use `log()` to show console some info
```cpp
string s = "a";
log("string s is %s", s);
```

# Details

## Sprite

### Create
- can use a `.jpg` or `.png` or `.tiff` or `.webp` image 
```cpp
// using the whole, raw (same resolution) pic
auto mySprite = Sprite::create("mysprite.png");

// using a rectangle to cut out a piece of the raw pic
auto mySprite = Sprite::create("mysprite.png", Rect(0, 0, 40, 40));
// the rect's params : sttx, stty, length, height
```

- can use a image set (Sprite Sheet)
sprite sheet : using particular tools to merge pictures into a big one, using formats like `plist` to index, costing less spaces, giving better performance.
When using a Sheet, first load them into `SpriteFrameCache`, a global cache class holding `SpriteFrame` objects, boosting access, these objects are load once and kept in `SpriteFrameCache`
```cpp
// load the sprite sheet
auto spritecache = SpriteFrameCache::getInstance();

// the .plist is generated by other tools
spritecache -> addSpriteFramesWithFile("sprites.plist");
```

- can use sprite cache
```cpp
// grab the sprite named "mysprite.png" from the .plist
// create the sprite and cram it into SpriteFrameCache
auto mysprite = Sprite::createWithSpriteFrameName("mysprite.png");

// to access a sprite from the cache obj SpriteFrameCache
// first get the SpriteFrame from the cache obj
// and create the sprite from the SpriteFrame
auto newspriteFrame = SpriteFramCache::getInstance() -> getSpriteFrameByName("mysprite.png");
auto newSprite = Sprite::creatWithSpriteFrame(newspriteFrame);
```

### Manip

- scale
```cpp
mySprite -> setScale(2.0);
mySprite -> setScaleX(2.0);
mySprite -> setScaleY(2.0);
```

- skew
```cpp
mySprite -> setSkewX(20);
mySprite -> setSkewY(20);
```

- color
```cpp
// set the color by passing in a pre-defined Color3B object.
mySprite -> setColor(Color3B::WHITE); // White

// Set the color by passing in a Color3B object.
mySprite -> setColor(Color3B(255, 255, 255)); // Same as Color3B::WHITE
```

- opacity
```cpp
mySprite -> setOpacity(30);
// the larger the number, the more concrete
```

### Polygon Sprite
*normal sprite is seen as two triangles when plotting, while a polygon sprite is seen as a series of triangles*
- promote the performance

- use AutoPolygon : a tool class measuring a image to split it into triangles
```cpp
//first feed the image into the AutoPolygon, get info
auto pinfo = AutoPolygon::generatePolygon("mySprite.png");
//and create sprite with the info
auto sprite = Sprite::create(pinfo);
```

## Actions

### Basic Actions
```cpp
auto mspt = Sprite::create("mySprite.png");
```
- move
```cpp
auto moveTo = MoveTo::create(2, Vec2(50, 0));
auto moveBy = MoveBy::create(2, Vec2(50, 0));
mspt -> runAction(moveTo);
mspt -> runAction(moveBy);
```
- rotate
*clockwise*
```cpp
auto rotateTo = RotateTo::create(2.0, 40); // over 2 seconds
auto rotateBy = RotateBy::create(2.0, 40);
mspt -> runAction(rotateTo);
```
- scale
```cpp
//scale x by 3, y by 4 in 2 sec
ScaleBy::create(2.0, 3.0, 4.0);
//scale to 3 uniformly in 2 sec
ScaleTo::create(2, 3);
```

- fadein & fadeout
```cpp
//in 1 sec
FadeIn::create(1.0);
FadeOut::create(2.0);
```

- tint
mix color for a node object who support `NodeRGB` protocol
```cpp
//tints a node to the specific RGB values
auto tintTo = TintTo::create(2.0, 120.0, 232.0, 254.0);

//tints a node by the specific RGB values
auto tintBy = TintBy::create(2.0, 120.0, 232.0, 254.0);
```

- frame anime
```cpp
auto mySprite = Sprite::create("mysprite.png");

// now lets animate the sprite we moved
Vector<SpriteFrame*> animFrames;
animFrames.reserve(12);
animFrames.pushBack(SpriteFrame::create("Blue_Front1.png", Rect(0,0,65,81)));
animFrames.pushBack(SpriteFrame::create("Blue_Front2.png", Rect(0,0,65,81)));
animFrames.pushBack(SpriteFrame::create("Blue_Front3.png", Rect(0,0,65,81)));
animFrames.pushBack(SpriteFrame::create("Blue_Left1.png", Rect(0,0,65,81)));
animFrames.pushBack(SpriteFrame::create("Blue_Left2.png", Rect(0,0,65,81)));
animFrames.pushBack(SpriteFrame::create("Blue_Left3.png", Rect(0,0,65,81)));
animFrames.pushBack(SpriteFrame::create("Blue_Back1.png", Rect(0,0,65,81)));
animFrames.pushBack(SpriteFrame::create("Blue_Back2.png", Rect(0,0,65,81)));
animFrames.pushBack(SpriteFrame::create("Blue_Back3.png", Rect(0,0,65,81)));
animFrames.pushBack(SpriteFrame::create("Blue_Right1.png", Rect(0,0,65,81)));
animFrames.pushBack(SpriteFrame::create("Blue_Right2.png", Rect(0,0,65,81)));
animFrames.pushBack(SpriteFrame::create("Blue_Right3.png", Rect(0,0,65,81)));

// create the animation out of the frames
Animation* animation = Animation::createWithSpriteFrames(animFrames, 0.1f);
Animate* animate = Animate::create(animation);

// run it and repeat it forever
mySprite->runAction(RepeatForever::create(animate));
```

- move at variable speed
using variable speed motion to simulate physic phenonmena to lessen the cost
```cpp
// create a sprite
auto mySprite = Sprite::create("mysprite.png");

// create a MoveBy Action to where we want the sprite to drop from.
auto move = MoveBy::create(2, Vec2(200, dirs->getVisibleSize().height -
 newSprite2->getContentSize().height));

// create a BounceIn Ease Action
auto move_ease_in = EaseBounceIn::create(move->clone() );
auto move_ease_in_back = move_ease_in->reverse();

// create a delay that is run in between sequence events
auto delay = DelayTime::create(0.25f);

// create the sequence of actions, in the order we want to run them
auto seq1 = Sequence::create(move_ease_in, delay, move_ease_in_back,
    delay->clone(), nullptr);

// run the sequence and repeat forever.
mySprite->runAction(RepeatForever::create(seq1));
```

### Clone & Rev

- an action when run would change its state, cloning one would give a same but brand new action, ==clone won't copy the usage state!==

```cpp
sprite1 -> runAction(sequence -> reverse());
sprite1 -> runAction(sequence);
//get back
```

## Scene
```cpp
auto myScene = Scene::create();
myScene -> addChild(mySprite);
```
- coordinates are calculated from the bottom-left point

### switch between scenes

- `runWithScene`
to start the game with the ==first== scene
```cpp
Director::getInstance() -> runWithScene(myScene);
```

- `replaceScene`
to kill the current scene
with the referred scene
```cpp
Director::getInstance() -> replaceScene(myScene2);
```

- `pushScene()`
to pause the current scene and push it into the scene stack
and show the referred scene
```cpp
Director::getInstance() -> pushScene(myScene3);
```

- `popScene()`
to kill the current scene
show the top scene of the stack, and pop()
if empty(), kill the app

### effects added to replacing

```cpp// Transition Fade
Director::getInstance() -> replaceScene(TransitionFade::create(0.5, myScene, Color3B(0,255,255)));

// FlipX
Director::getInstance() -> replaceScene(TransitionFlipX::create(2, myScene));

// Transition Slide In
Director::getInstance() -> replaceScene(TransitionSlideInT::create(1, myScene));
```

> here skipped the UI and Special Nodes these two chaps

