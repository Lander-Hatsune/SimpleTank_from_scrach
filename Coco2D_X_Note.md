# Coco2D-X Notebook
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
mySprite -> set AnchorPoint(Vec2(0, 0)); // referenced base point on this sprite when calculating its coordinates
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
