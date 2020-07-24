# TextButton. General usage.

## Create.

`TextButton tb = new TextButton("Text on the Button", textButtonStyle);`

## Set font scale.

`tb.getLabel().setFontScale(2);`

## Add ChangeListener.

```
tb.addListener(new ChangeListener() {
                    @Override
                    public void  changed(ChangeEvent event, Actor actor) {
                        functionWeWantToStartOnTheEvent();
                    }
                });
```