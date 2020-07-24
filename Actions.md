### Libgdx scene2d actions ###

**credits**
All credits for this goes to Ludevik. He wrote up a wonderful tutorial over at stack overflow  http://stackoverflow.com/questions/9131554/actions-of-actors-in-libgdx

I though it would be better to have on the official wiki

## DEPRECATED ##
**OBSOLETE**
This page is no longer valid for the current LibGDX build, see [Scene2d Actions section](https://github.com/libgdx/libgdx/wiki/Scene2d#actions) instead.

## Actions Overview ##
There are various available actions in LibGDX ready for you. They are in com.badlogic.gdx.scenes.scene2d.actions package. I would say that there are 3 kinds of actions:

### Animation actions ###
Animation actions modify various properties of your actor, such as location, rotation, scale and alpha. They are:

| * action * | * description * |
|:-------------:|:----------------|
| * FadeIn * | changes alpha of your actor from actor's current alpha to 1|
| * FadeOut *  | changes alpha of your actor from actor's current alpha to 0|
| * FadeTo *  | changes alpha of your actor from actor's current alpha to specific value|
| * MoveBy *  | moves your actor by specific amount|
| * MoveTo *  | moves your actor to specific location|
| * RotateBy *  | rotates your actor by specific angle|
| * RotateTo *  | rotates your actor to specific angle|
| * ScaleTo *  | scales your actor to specific scale factor|



### Composite actions ###
Composite actions combine multiple actions in one action, there are:

| * action * | * description * |
|:-------------:|:----------------|
| * Parallel * | execute given actions in parallel - all actions at once |
| * Sequence * | execute given actions in sequence - one after another |


### Other actions ###
These should be fairly self explainatory.

| * action * | * description * |
|:-------------:|:----------------|
| * Repeat * | repeats given action n-times |
| * Forever * | repeats given action forever |
| * Delay * | delays execution of given action for specific amount of time |
| * Remove * | removes given Actor from stage |


**Every action has a static method $ which creates instance of that Action. **

## Example of creating animation actions ##

```java
MoveTo move = MoveTo.$(200, 200, 0.5f); //move Actor to location (200,200) in 0.5 s
```
```java
RotateTo rotate = RotateTo.$(60, 0.5f); //rotate Actor to angle 60 in 0.5 s
```

## Example of creating more complex action sequence ##

```java
Sequence sequence = Sequence.$(
             MoveTo.$(200, 200, 0.5f), //move actor to 200,200
             RotateTo.$(90, 0.5f), //rotate actor to 90°
             FadeOut.$(0.5f), //fade out actor (change alpha to 0)
             Remove.$() //remove actor from stage
);
```

## Animation Interpolator ##

Animation actions also let you specify Interpolator. There are various implementations:

* **AccelerateDecelerateInterpolator** 
* **AccelerateInterpolator**
* **AnticipateInterpolator** 
* **DecelerateInterpolator** 
* **LinearInterpolator** 
* **OvershootInterpolator** 

_Interpolator Javadoc: An interpolator defines the rate of change of an animation. This allows the basic animation effects (alpha, scale, translate, rotate) to be accelerated, decelerated etc. To set interpolator to your action_

```java
action.setInterpolator(AccelerateDecelerateInterpolator.$());
```

When you have your action with interpolator ready, then you set that action to your actor:
```java
actor.action(yourAction);
```
To actually execute all actions defined for actors on stage, you have to call stage.act(...) in your render/update method:

```java
stage.act(Gdx.graphics.getDeltaTime());
stage.draw();
```