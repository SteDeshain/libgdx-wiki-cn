use a [[Spritebatch, Textureregions, and Sprites]]

# TextureRegion

`TextureRegion`s can be created without a texture. This is useful if a region should be populated with a texture at another point in time. Doing so however bears the risk of a **NullPointerException** if this initialization is not done and the region is drawn. To prevent this, draw calls can be protected:

```java
private SpriteBatch spriteBatch = SpriteBatch();
private TextureRegion textureRegion = new TextureRegion();

@override
public void render(float deltaTime)
{
  spriteBatch.begin();
  if(textureRegion.getTexture() != null) 
  { 
    spriteBatch.draw(textureRegion, 0f, 0f, 1024f, 512f); 
  }
  spriteBatch.end();
}
```