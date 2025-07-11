
# Users and Members

## Users
Table to manage user logins and authentication, with typical tables like Roles, Permissions etc..
User has a MemberID which is his default MemberID as a player

## Members  and groups
Members are players or player groups defined by **member_types**
when user first logs in a member of type player is created. This is unique and is the default member.
Player can then create additional grouptype members 
Some member features are available to player or group members only.
Members can create groups of members and define roles within group (to be extended)
Player members can create avatars
Members can purchase items by adding them to baskets (wishlist or custom names) 
then paying the basket content through a transaction of type assetpurchase
This will add the items to the **member_items** and define the **asset licence type**

A member can create **Items**, ownership is defined in the **MemberID** of the Item
A member can create **badges** and give them to players within **ExperienceItems**

Members can have **relations** (different for groups or players) like group member, follower etc...
Member can define its Player or Creator **skill level**
## MemberPlayers
MemberPlayers  can:
- Create Avatars
- Create groups (max 5, paid 100)
- Join  groups
- Become Friends with others
- Follow groups or players
- Buy Credits 
- Buy Assets for individual use
- Buy and trade limited items
- Join dedicated servers
- Post on forums
- Send messages to other players

## MemberGroups
MemberGroups  can:
- Create roles and ranks for group members (different than User roles/permissions...)
- Access and Distribute Group Credits to members
- Create group Alliances and enemies
- Create Badges
- Create Experience passes
- Post Messages on group MainPage
	

## All Members (Player or Group)  
All Members can:
- Create Projects, Assets and Experiences
- Sell Assets and Experiences on Marketplace
- Create and sell limited items
- Buy Assets for projects
- Earn Credits
- Distribute Credits
- Create and host events
- Advertise Experiences and Assets
- Host dedicated Servers



Also see
[Group Roles/Ranks and Permissions – Roblox Support](https://en.help.roblox.com/hc/en-us/articles/203313770-Group-Roles-Ranks-and-Permissions)



# Items

Items are  sellable objects created by the Member: Experiences, 3d objects, textures... (item categories)
Items owned by members are listed in Member_Items table, not here

MemberID is the creator of Item (player or group)
**Item tags** are additional categories for items
**Item_state** defines the state of item: project, alpha, release...
**Item_versions** are releases of item and download paths (notes are internal)
**Item groups** are for sales of item bundles, feature optionally a relative transform 
**Item place data** is optional data for placed items such as cities and landmarks

**Item Sales** are used to sell items, sale types are regular, special, etc...
**Licence types** are related to asset usage  (free, personal, professional) see FAB
Sales can be in CREDITS, USD, EUR, GBP rest of currencies are converted

Once an item is ready for sale it can be published in one or more stores
**Stores** Are places where the asset will be sold
Member Stores are visible only within Experiences (Games) . By default only free and personal use prices are visible there

A special Store is the Main Airland Store where all licenses are visible, we do not create a separate table as we might have to create substores within the website (i.e. Creator Store, Premium members stores etc..)

Items are linked to Stores  by a many to many table between Items and Stores, where Member can also define if all licenses and sales are visible (null values on columns) or 




# Content tables for Items or Members
Content tables are available for Members or Items
They allow to publish content in different contexts (content_category) (member or product page, asset descriptions)
can be of different format (text, images, video, 3d...)

We will use this table for mostly for text content (i.e. Item descriptions, group descriptions etc...) but also member videos photos etc...

If needed text content will then be auto translated and translation saved in Language_Translations table 

# Member Baskets
Baskets are used for wishlists and actual purchases, member can add items to baskets and the state  of the basket defines its state as wishlist or actual purchase

When a purchase is done
We run the transaction as explained below, if the Transaction is completed we do the following:

Update the Basket with the TransactionID

the relevant columns if Items present in the Member_Basket_Details  are copied into the Member_Items table and Items become available to user

An invoice is issued to the Member writing into Member_Invoices Table

# Member Transactions

Transactions are monetary or credit exchanges between members
When a transaction is initiated we create a new transaction on the Member_Transactions table registering the 2 members involved, the transaction date and the transaction type from Member_transaction_types table

We retrieve the transaction ID 
Verify if Buyer has enough Credits to complete the transaction 
Register the transaction in the Member_Transactions Table or Member_Currency_Transactions table,
using a negative sign for the BuyerID 
If process is OK we update the transactionCompleted date on Member_Transactions table


# Member Messages
Simple messaging system that allows to exchange messages and post comments on Forums, products etc...

A message is created by a member and can be sent to one or more members using the Member_Message_Recipients table

it can also be sent to a member in reference to an ItemID this allows comments on Member Items

Replies to messages register in the Member_Messages saving the ID of the message being replied in the RepliedMessageID column




