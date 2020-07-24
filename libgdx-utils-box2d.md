# Overview #

libgdx-utils-box2d is a small support library for games and applications powered by libGDX and its Box2D extension. It provides tools for various problems to save you time and effort.

The project is hosted on [bitbucket](https://bitbucket.org/dermetfan/libgdx-utils/wiki).

# API Reference #

- [Javadoc](http://dermetfan.net/libgdx-utils/box2d/javadoc)

## Graphics ##

- [**Box2DSprite**](https://bitbucket.org/dermetfan/libgdx-utils/wiki/net.dermetfan.gdx.graphics.g2d.Box2DSprite) - a Sprite that draws itself on bodies and fixtures
- [**AnimatedBox2DSprite**](https://bitbucket.org/dermetfan/libgdx-utils/wiki/net.dermetfan.gdx.graphics.g2d.AnimatedBox2DSprite) - should be rather self-explanatory
- [Box2DPolygonSprite](https://bitbucket.org/dermetfan/libgdx-utils/wiki/net.dermetfan.gdx.graphics.g2d.Box2DPolygonSprite) - like the Box2DSprite but for PolygonRegions

## Miscellanious ##

- [Box2DUtils](https://bitbucket.org/dermetfan/libgdx-utils/wiki/net.dermetfan.gdx.physics.box2d.Box2DUtils) - various operations and information on bodies, fixtures and shapes
- [**Box2DMapObjectParser**](https://bitbucket.org/dermetfan/libgdx-utils/wiki/net.dermetfan.gdx.physics.box2d.Box2DMapObjectParser) - creates Box2D scenes from a map (e.g. [Tiled](http://mapeditor.org/))
- [Breakable](https://bitbucket.org/dermetfan/libgdx-utils/wiki/net.dermetfan.gdx.physics.box2d.Breakable) - easy way to create breakable bodies, fixtures and joints
- [Chain](https://bitbucket.org/dermetfan/libgdx-utils/wiki/net.dermetfan.gdx.physics.box2d.Chain) - manages, builds and modifies chain/rope simulations
- [**MouseJointAdapter**](https://bitbucket.org/dermetfan/libgdx-utils/wiki/net.dermetfan.gdx.physics.box2d.MouseJointAdapter) - create managed mouse joints in seconds
- Autopilot - moves and rotates bodies to a destination
- ContactMultiplexer - notifies multiple ContactListeners