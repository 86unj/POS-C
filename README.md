# Project: Simple Point Of Sale System

A simple Point of Sale (POS) application that keeps track of a small inventory of Goods to sell and can sell them at the cashier, issuing a bill of sale. 


### Milestones

|Milestone| | Overview | Comments |
|------|:---:|:---------|:------|
| [MS1](#milestone-1) | | POS UI | Create a mock-up application |
| [MS2](#milestone-2-inventory-listing) | | Inventory listing | Adding functionality to the POS system |
| [MS3](#milestone-3) | | Point of Sale |
| | [MS31](#ms31--void-saveitemsconst-char-filename) | Save Item |
| | [MS32](#ms32-double-billdisplayconst-struct-item-item) | Display Bill |
| | [MS33](#ms33-void-displayconst-struct-item-item) | Print Item |
| | [MS34](#ms34-int-searchvoid) | Search Item  |  
| | [MS35](#ms35-void-posvoid) | POS |
| [MS4](#milestone-4-stock-item) |    | Stock Item |
| [MS5](#milestone-5-add-and-remove) |   | Add and Remove Items | 
 

# Milestone 1

## `POS.h` header file

Create a file called `POS.h` and add the following defined values to be used in the POS application:

- TAX  `0.13`
- MAX_SKU_LEN  `6`
- MAX_STOCK_NUMBER  `999`
- MAX_NO_ITEMS  `500`
- MAX_BILL_ITEMS `10`

## The PosUI Module (Point of Sale User Interface)

This Module contains one Module called `PosUI`. This module is responsible to offer the user a menu of features provided by the POS system. 

The user, then, can select an option representing the feature and execute it. After the execution is complete, the system displays the menu again, until the user selects to exit the application.

### `utils` Module
The utils Module (that is fully implemented) is the module you have developed throughout the semester in your workshops. This module contains basic fool-proof user input functionalities. Use this module for data numeric data entry in your project. 

### The `PosApp` module 
This module is already created and contains the mock-up functions of what needs to be done in the POS system.  Call these functions in your PosUI module when the user selects different POS menu options.

### The features of the POS system

1. Inventory  
2. Add Item
3. Remove Item
4. Stock Item
5. Point Of Sale


### Implementation

For milestone 1 you will create a mock-up module for the Point Of Sale that will demonstrate how the system is going to run (eventually) by only printing the name of the actions, instead of executing them. In later stages of development, you will replace these messages with the proper logic to actually perform the action. 

Note that the signature of many of the functions created now may be changed to accommodate what needs to be done later.

#### The `PosUI` functions

In the `PosUI` module, you will create two functions: `menu` and `runPos`

##### `int menu(void)` 

Write a function that displays the name of the application and the other options to select in the application.<br />
This function will display the menu of the system, receive the user's choice (in a foolproof way, *hint*: use your utils) and returns the choice. See below:

```text
The Sene-Store
1- Inventory
2- Add item
3- Remove item
4- Stock item
5- POS
0- exit program
> a
Invalid Integer, try again: -1
[0<=Selection<=5], retry: 6
[0<=Selection<=5], retry: 1
>>>> Inventory...
```

##### Mockup functions:

the following 7 mock-up functions are provided in the `PosAp` module that only print `>>>>> ` and their names. 

1. inventory()
2. addItem()
3. removeItem()
4. stockItem()
5. POS()
6. loadItems(filename)
7. saveItem(filename)

Call the above functions in the `runPos` function when corresponding option is selected by the user.

##### `void runPos(const char filename[])` 

receives the name of the data file and returns void.

> implement the following actions calling the corresponding mockup methods

This method first loads all the item records and then displays the menu waiting for the user to make the selection. After the (foolproof) selection, the proper action is executed, then the menu is displayed again until the 'exit' option is selected. When the 'exit' option is selected, all the records are saved and a `Goodbye!` message is displayed.





# Milestone 2 (Inventory listing)

For your milestone 2 implement the first menu option of the application.

## Item definition and the data array

Add a structure for an `Item` to the `PosApp.h` header file to hold a record for an Item to be sold using the POS system.

An item has the following:

- SKU (Character C-string with a maximum length of MAX_SKU_LEN )
- name (Character C-string with a maximum length of 60)
- price (double)
- taxed (an integer flag; true or false)
- quantity (integer)

> *tip*: maximum length refers to the amount of 'readable characters' the c-string has.

Add a global array of `Item` structure type and set the number of elements that it can contain to: the `MAX_NO_ITEMS`. This will be important when storing the items read from the file into our application. (we will call this array `items` array from now on)

Add a global integer variable to hold the number of records read from the file (we call this integer `noOfItems` from now on)

## Inventory listing

The Inventory listing component of our application starts with a call to the `void inventory(void)` function. To implement this function you need to create 3 helper functions first:

#### 1- `double cost(const struct Item* item)`

This function receives an address of an item structure and returns the cost of that item after tax using the following calculation:
 
`the_cost = price * (1+ taxed* TAX)`

- the_cost: value to be returned
- price: the price of the item
- taxed: The tax flag that is either true(1) or false(0)
- TAX: defined in `POS.h`

As a side note, consider why we pass pointers to an item structure as opposed to passing an item structure by value.

#### 2- `int LoadItems(const char filename[])`

This function loads all the items from a data file (the name received from the argument) into the `items` array and sets the `noOfItems` variable to the number of records read.

The records are in the following format:  
- SKU
- comma (`,`)
- item name
- comma (`,`)
- price 
- comma (`,`)
- taxed or not (0 or 1)
- comma (`,`)
- quantity
- newline

To accomplish this do the following:

- print `>>>> Loading Items...` *hint*: reuse logic that you've coded already
- In a `File*` variable open the `filename` for reading
- If the opening is successful (The file pointer is not null)
    - in a loop read all the items of the file into the `items` array
    - set `noOfItems` to the number of records read
- print `>>>> Done!...`
- return the `noOfItems`
- For a refresher on working with files, visit: https://intro2c.sdds.ca/E-Secondary-Storage/records-and-files 

#### 3- `void listItems(void)`

A display function that lists the items in the `items` array up to `noOfItems`.

To accomplish this do the following:

Create a local c-string for printing name (18 readable characters+ 1 for the null byte), let's call this `iName`

- print the header:
```text
 Row | SKU    | Item Name          | Price |TX | Qty |   Total |
-----|--------|--------------------|-------|---|-----|---------|
```
- in a loop starting from zero up to `noOItems`, go through the elements of the `items` array
    - copy the first 18 characters of the `item` element into `iName`
    - print row number (loop index + 1) under  (in 4 spaces)
    - print `" | "`
    - print the SKU  (in 6 spaces)
    - print `" | "`
    - print the `iName` (in 18 spaces left-justified)
    - print `" |"`
    - print the price  (in 6 spaces 2 digits after the decimal point)
    - print `" | "`
    - print if it is taxed (`T` if is taxed or space if not) *hint*: try doing this all in one printf using inline calculations
    - print quantity (in 3 spaces)
    - print `" |"`
    - print total price using the following inline calculation: ([cost](#1--double-costconst-struct-item-item) * quantity in 8 spaces 2 digits after the decimal point )
    - print `" |\n"`
- print the footer
```text
-----^--------^--------------------^-------^---^-----^---------^
```
### `void inventory(void)` 
Now that the 3 helper functions are done, use the three functions implement this function as follows to:

- Create a double variable for total asset value (let's call it `tav`) and set it to zero
- Print `>>>> List Items...`
- Call `listItems()`
- Calculate the total asset value (`tav`) by looping through the `items` elements and accumulating the `cost * quantity` of each element in `tav`; 
- print the total asset value in a footer in the following format:
```text
                               Total Asset: $  | 9999999999.99 |
-----------------------------------------------^---------------^
```





## MS31  `void saveItems(const char filename[])`

Create a function in PosApp.c module called saveItems, this function receives a filename and saves all the elements of the `items` array in the file in a comma-separated format identical to the `posdata.csv` file.

- open the file (using the `filename` argument) for writing. 
- If the opening was successful...
    - in a loop that goes up to `noOfItems`, write all the variables of the `items` elements in a comma-separated format. Make sure each record is separated from the next one with a new-line character(`\n`). 
    - close the file
- If the opening was not successful print `Could not open >>?????<<\n` replace `?????` with the file name 




## MS32 `double billDisplay(const struct Item* item)`

This function will be used in POS to print a bill. 

- Create a temporary cString with a size of 14 readable characters + 1 for the null-byte
- Display the item's name, cost, and if the item is taxed as follows;
    - print `"| "`
    - print the first 14 characters of the name in 14 spaces left justified
    - print `"|"`
    - print the cost of the item (price after tax, in 10 spaces with 2 digits after the decimal point)
    - print `" | "`
    - print `Yes` if the item is taxed or three spaces if not
    - print `" |\n"`
- return the cost



## MS33 `void display(const struct Item* item)`


This function is used to print an Item in a "Form" (i.e non-linear, detailed) format.

- print `=============v`
- print `Name:`
- print the name aligned with the `v` of the first line
- print `Sku:`
- print the SKU aligned with the `v` of the first line
- print `Price: `
- print the price aligned with the `v` of the first line with 2 digits after the decimal point
- print `Price + tax:`
- print the `cost()` with 2 digits after the decimal point or `N/A` if the Item is not taxed, aligned with the `v` of the first line  
- print `Stock Qty:`
- print the quantity aligned with the `v` of the first line
- print `=============^`
- go to a new line







## MS34 `int search(void)`

The search function asks for an SKU entry from the user and loops and searches through the `items` array for the SKU entered. 

1. if the SKU is found in the `items` array, the index of the item holding the `SKU` will be returned.
2. if the SKU is not found in the `items` array, -1 is returned.
3. if the user does not enter any SKU and just hits enter, -2 is returned. 

- Prompt the user with `Sku: `
- Scan the Sku in a local variable 
- flush the keyboard if needed. 
- loop through the `items` array and search for the SKU
- After the loop return one of the stated above options in 1, 2 or 3. 

> Note: a function with a good coding style should have only one `return` statement.

### Sample executions for the following code snippet:

```C
int retValue = search();
printf("The search function returns: %d", retValue);
```

- Assuming there is an item with the SKU `1234` in the `items` array at the index 20:
```
SKU: 1234<ENTER>
The search function returns: 20
```
- Assuming there is no item with the SKU `1234` in the `items` array:
```
SKU: 1234<ENTER>
The search function returns: -1
```
- Assuming the user only hits enter:
```
SKU: <ENTER>
The search function returns: -2
```





## MS35 `void POS(void)`

The POS function is the most important in the application. 

This function can be implemented in two different ways; 
1. Uses an array of Item structures to keep copies of the sold items for bill printing (uses more memory)
2. Uses an array of Item structure pointers to keep the addresses of the sold items for bill printing (uses less memory )

You can implement your POS function either way. The first version is less complicated and receives a full mark. The second way uses an array of pointers whose concept is not covered in IPC144 and is more complicated, hence receiving bonus marks. I recommend doing the first version and if successful, converting it to the second version later.
> Note that you may be asked to defend or explain your method of implementation. 

### Version 1: less complicated 
The function POS in a loop using the search() function, asks the user for an SKU and then if the Item is found it will reduce the quantity of the item by one and then adds a copy of the item to a local array of Items (let us call this the `bill` Item array) and increases the value of a local integer variable to keep track of the items added to the bill. This integer number should be less than the `MAX_BILL_ITEMS` values, if the number reaches the `MAX_BILL_ITEMS` then the function ends by printing the bill.

If the item is not found the message `"SKU not found!\n"` is printed.

If the item is found but the quantity of the Item is zero, the message `"Item sold out!\n"` is printed.

If the user does not enter an SKU and instead only hits `<ENTER>` the POS function ends by printing the bill.


### Version 2: more complicated (bonus marks)
If you have completed this bonus section you must indicate it in the `reflect.txt` file to get bonus morks.

The function POS in a loop using the search() function, asks the user for an SKU and then if the Item is found it will reduce the quantity of the item by one and then adds the address of the item to a local array of Item pointers (let us call this the `bill` Item pointer array) and adds one to a local integer variable to keep track of the items added to the bill. This integer number should be less than the `MAX_BILL_ITEMS` defined value, if the number reaches the `MAX_BILL_ITEMS` then the function ends by printing the bill.

If the item is not found the message `"SKU not found!\n"` is printed.

If the item is found but the quantity of the Item is zero, the message `"Item sold out!\n"` is printed.

If the user does not enter an SKU and instead only hits `<ENTER>` the POS function ends by printing the bill.


### printing the bill
Print the bill by first printing the bill title:
```text
+---------------v-----------v-----+
| Item          |     Price | Tax |
+---------------+-----------+-----+
```
Then in a loop go through the local `bill` array and print each element using the `billDisplay` array. Meanwhile, in a local double variable, accumulate the total bill price to be printed later.

When done close the bill by printing a footer containing the total price as follows:
```text
+---------------^-----------^-----+
| total:          999999.99 |
^---------------------------^
````




# Milestone 4 (Stock Item)

Implement the option 4 of the POS system to add to the quantity of the Items to the maximum of `MAX_STOCK_NUMBER`.
> Note: This output is only a sample for debugging purposes; the tester 
      output may have different values
      

### Suggestions implementation
This is an open bonus section. You can implement it any way you like but if asked, you should be able exactly how your code works.

If you like some suggestions for implementation, here they are:

1. Reuse your `listItems()` function to create a function called `selectItems` that receives nothing and returns an integer.  This function should display a header for the items with a message to prompt the user to select an item and then do a full proof entry between 1 and the number of items in the inventory and return that value. This function will be used in the next function to let the user select an item to add to its quantity.
2. Implement the `StockItem()` function by first letting the user select an Item, then do a full proof entry for an integer between 1 and the difference between the current quantity of the item and the `MAX_STOCK_NUMBER`. Add that number to the quantity of the item.




# Milestone 5 (Add and Remove)
This is an open bonus section. You can implement it any way you like but if asked, you should be able exactly how your code works.

For milestone 5 set the `MAX_NO_ITEMS` defined value to `28`.

Implement the add (option 2) and the remove (option 3) functionalities to the POS system as follows:

## Add
Perform a foolproof data entry and receive the information of a new Item and add it to the `items` array as shown in the execution sample.

## Remove
Show the user the list of all the items and let the user select one, then remove it from the `items` array.

      

