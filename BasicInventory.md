# Inventory Tutorial
> This tutorial is for a basic inventory system with Item assets that you can add to inventory slots.

### 1. Create a new scene: Setting everything up
> Begin by creating a new scene in Unity called Inventory.
> Create a new script called “Inventory”, a script called “InventorySlot”, and a script called “Item”.
> Create a folder and call it “Items”.
> Create a canvas. In the canvas settings, change the render mode to “Screen space - Camera”. Drag the camera into the camera slot that appears below.
> Add a button to the canvas, put it near the upper left corner of the screen and change the text to say “Add Item”. Name the button "AddItem".
> Create a new panel on the canvas. Scale its size until it only fills most of the right half of the screen. Name it "InventoryPanel". Add a Grid Layout Group to it.
> Create an empty game object and call it “InventoryHandler”. Drag the inventory script onto it in the hierarchy.
### 2. The Item Script
> Open up the item script. Delete the Start and Update functions. The Item script is only going to be an asset to hold data, so we don’t need it to derive from Monobehaviour. Instead, in the class declaration, write
```C#
public class Item : ScriptableObject
```

> We are going to declare several variables here that will be necessary for each Item asset: 
```C#
public string itemName = "New Item";
public string itemDescription = "New Description";
public Sprite icon;
public int price = 0;
public enum Type { Default, Consumable, Weapon, Ammunition }
public Type type = Type.Default;
```
> Most of these are pretty self-explanatory. We need a Type declaration to tell us what type of item that a certain item will be so that later on, when reading the data, we know more about the item. We need a price variable in case we want to create a buying/selling system later on.
> The last thing that we need to add is this line of code before the class declaration:
```C#
[CreateAssetMenu(fileName = "New Item", menuName = "Inventory/Item")]
```
> This will make it so that we can create items as an asset from the create menu in Unity. It gives each item asset the default name of “New Item” and tells Unity where to create the menu to make one.

### 3. The Inventory Script

##### Variables
> Next we're going to write the inventory script. Open the Inventory script. This script doesn't need to do anything at runtime so you can delete the start and update functions.
> Add the following variables to the beginning of the script:

```C#
public Item[] itemList = new Item[20];
public List<InventorySlot> inventorySlots = new List<InventorySlot>();
```

> The itemList is a the list of Item assets that will be read into the Inventory slots. We want the itemList to be an array so that we can remove items later without the size of the list decreasing. The inventorySlots is a list because each slot will correspond to a gameObject in the UI in Unity.

##### The Add functionality
> We need to be able to add items to our inventory. To accomplish this, we will add the following three functions to the script:

```C#
private bool Add(Item item)
	{
		for (int i = 0; i < itemList.Length; i++)
		{
			if (itemList[i] == null)
			{
				itemList[i] = item;
				inventorySlots[i].item = item;
				return true;
			}
		}
		return false;
	}

	public void UpdateSlotUI()
	{
		for(int i = 0; i < inventorySlots.Count; i++)
		{
			inventorySlots[i].UpdateSlot();
		}
	}

	public void AddItem(Item item)
	{
		bool hasAdded = Add(item);
		if(hasAdded)
		{
			UpdateSlotUI();
		}
	}
```

> The Add function will cycle through all of the items in the itemList and find the first available slot. It returns a boolean so we know if it successfully added the item. If it finds an opening in the list, then it returns true and ends its search through the list there. If it gets to the end of the loop, that means that it didn't find an opening. This indicates that the item was not added so it returns false.

> The AddItem function uses a boolean hasAdded to call the Add function. If it was added successfully, then it calls the UpdateSlotUI function to reflect the changes. Otherwise, nothing happens.

> The UpdateSlotUI function cycles through all of the inventorySlots and calls the updateSlot function in each. This brings us to

### 4. The InventorySlot Script
##### Variables
> Open the InventorySlot script. Add the following variables to the beginning of the script:
```C#
public Item item;
public GameObject icon;
```
> The item variable corresponds to the item that is in the inventory slot. The icon variable is for the item's icon.
##### Update Functionality
> Next, delete the start and update functions. Replace them with this:

```C#
public void UpdateSlot()
	{
		if (item != null)
		{
			icon.GetComponent<Image>().sprite = item.icon;
			icon.SetActive(true);
		}
		else
		{
			icon.SetActive(false);
		}

	}
```
> The updateSlot function checks to see if its item slot has an item in it. If it does, it sets its icon to be active and sets its icon to the icon associated with its item. If it doesn't have an item, the icon is deactivated to show an empty UI slot.

### 5. Putting it all together

##### Creating Items
> Go back to the Unity editor. Open the items folder and right click. Go to Create > Inventory> Item. Do this twice.

> For the first item,  name it "BasicSword", set it to type "Weapon", add any description of a sword that seems fitting to you. Click on the circle next to the icon, and set it to the checkmark icon. Set the price to 30.

> For the second item, name it "MinorHealthPotion", set it to type "Consumable", add any description of a health potion that seems fitting to you. Click on the circle next to the icon, and set it to the knob icon. Set the price to 10.

##### Creating Inventory Slots
>  Right click on the InventoryPanel in the hierarchy and add an Image as its child. Name it "Slot (1)". Change its color to be a light brown.

> Click on the canvas, find the Canvas Scaler script in the inspector, and click on the dropdown for UI scale mode. Choose "Scale with Screen Size" and adjust the reference resolution until the image takes up about 1/6th of the size of the inventory panel. If the image is too small, you can click on the InventoryPanel's Grid Layout Group and adjust the size of the slot under "Cell size".

> Right click on the slot you created in the hierarchy and add an image as its child. Set its size in the rect transform to be about 80% of the size of its parent. Add the InventorySlots script to the Slot(1). Drag the child image of the slot into the Icon slot Slot(1)'s InventorySlot script.

> Duplicate the slot(1) until there are 20 slots altogether. They should be laid out in a grid. To adjust the spacing, go back into the InventoryPanel's Grid Layout Group and change the padding and spacing of the elements from the Inspector.

> Click on the InventoryHandler item, and click on the lock icon in the top right-hand side of the inspector. Find the Inventory Slots section of the Inventory script. Shift-select all of the inventory slots (Slot (1) - Slot (20)) and drag them into the Inventory Slots section. The list size should now be 20 and should include all of the inventory slots. You can now un-click the lock icon that you selected before.

##### Adding Items

> Click on the AddItem button. Scroll down to where it says "OnClick" at the bottom of the inspector. Click the plus sign and drag the InventoryHandler into the space that says "None". In the dropdown menu for the behavior, Click on Inventory>AddItem(Item). Finally, click on the circle next to the new empty space that says "None (Item)". Choose whichever item you would like to add to the inventory. This can be changed in unity at run-time to be any item.


##### Testing
> Test by clicking the play button and clicking The add item button. The item specified in the previous step is what will be added to the inventory.
