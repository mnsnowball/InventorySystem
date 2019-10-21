This tutorial is for a basic inventory system with Item assets that you can add to inventory slots.

### 1. Create a new scene: Setting everything up
###### Begin by creating a new scene in Unity called Inventory.
###### Create a new script called “Inventory”, a script called “InventorySlot”, and a script called “Item”.
###### Create a folder and call it “Items”.
###### Create a canvas. In the canvas settings, change the render mode to “Screen space - Camera”. Drag the camera into the camera slot that appears below.
###### Add a button to the scene, put it near the upper left corner of the screen and change the text to say “Add Item”.
###### Create a new panel on the canvas. Scale its size until it only fills most of the right half of the screen.
###### Create an empty game object and call it “InventoryHandler”. Drag the inventory script onto it in the hierarchy.
### 2. The Item Script
###### Open up the item script. Delete the Start and Update functions. The Item script is only going to be an asset to hold data, so we don’t need it to derive from Monobehaviour. Instead, in the class declaration, write
```C#
public class Item : ScriptableObject
```

###### We are going to declare several variables here that will be necessary for each item: 
```C#
public string itemName = "New Item";
public string itemDescription = "New Description";
public Sprite icon;
public int price = 0;
public enum Type { Default, Consumable, Weapon, Ammunition }
public Type type = Type.Default;
```

