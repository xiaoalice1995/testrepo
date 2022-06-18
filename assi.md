
# Assignment advanced programming in python
## Warehouse Management System
by Peiyu Xiao


### Rationale behind the design choices

#### Technical specifications

Development language: Python 3

Third-party library: this system does not contain third-party library.

Functionality:
- Add stock.
- Remove stock.
- Get number of stock of a certain type.
- Three types of stock should be defined.
- Provide a way of searching if stock is available.
- Create a report of all the stock available in the wearhouse.
- Provide input parameters and its functionality.

The warehouse management system is designed for two classes: _Stock_, _Warehouse_.

#### Architecture/execution model

- One base class for generic stock.

The stock management system class Stock is responsible for the storage of product related information. In `class Stock`, the basic data structure and type of Stock has been specified. The `__stock_type` list is private to store the name of stocks. The elements in the list are `product`. The quantity of each type is named `number`. Three types of stock are pre-defined.

``` python
class Stock:
    __stock_type = ["Kiwi", "Strawberry", "Papaya"] # create a list to record all the types that has been created
    def __init__(self, product: str, number: int) -> None:
        self.product = product
        self.number = number
        self.__stock_type.append(product)
                
    def stock_type(self) -> list:    
        return list(set(self.__stock_type))
```

- Use the generic class (inheritance) to define the different type of stock.

The `add_stock` operation is performed in the `class Warehouse`, and finally enters `Stock.product` through inheritance. Inheriting from the base class Stock, the functions `generate_id()` enables automatically generate warehouse id, and `update_stock_type()` ensure that user operations and information stored in the base class are updated in real time.

``` python
class Warehouse(Stock):    
    __wh_id = []

    def __init__(self) -> None:
        super(Stock, self).__init__()
        self.id = self.generate_id()
        self.storage = {}
        print(f"Warehouse {self.id} is initiated.")

    def generate_id(self) -> int:
        '''Generate id for warehouse'''
        if not self.__wh_id:
            self.__wh_id.append(1)
            return self.__wh_id[-1]
        else:
            self.__wh_id.append(self.__wh_id[-1]+1)
            return self.__wh_id[-1]
    
    def update_stock_type(self) -> None:
        '''To make sure the warehouse storage dict is up to date'''
        for product in self.stock_type():
            if product not in self.storage.keys():
                self.storage[product] = 0
```


- Main warehouse class provides the functionality.

The `class Warehouse` is responsible for the functional management of.

The System provides four functionalities: 
1. Add stock: call `add_stock()` enable users to add stocks by entering name and quantity, 
2. Remove stock: call `remove_stock()` enable users to remove stocks by entering name and quantity, 
3. Create a report of all the stock available in the wearhouse: call `get_all_stock_status()` allows user to print the storage dictionary with warehouse id information, and 
4. Get number of stock of a certain type: call `get_num_stock()` allows user to query stock information by entering name. 

``` python
def add_stock(self) -> None:
        '''Add an product and its number (quantity)'''
        assert self.number > 0, f"Only positive number allowed"
        self.update_stock_type()
        self.storage[self.product] += self.number

    def remove_stock(self) -> None:
        '''Remove stock by subtracting specified quantity of stock'''
        self.update_stock_type()       
        assert self.number > 0, f"Only positive number allowed"
        assert self.number <= self.storage[self.product], f"The storage is not enough"
        self.storage[self.product] -= self.number

    def get_all_stock_status(self) -> dict:
        '''Print the storage dict with Warehouse info'''
        self.update_stock_type()
        print("Warehouse ", self.id, self.storage)
        return self.storage

    def get_num_stock(self, name) -> int:
        ''' Query by the name '''
        self.update_stock_type()
        assert name in self.storage.keys(), "The item is not existed"
        print("The number of ", name, " in Warehouse ", self.id, " is ", self.storage[name])
        return self.storage[name]
```

The above functionality will be performed through the interactive function `Warehouse.ui()`. The `Warehouse.ui()` is responsible for user interface interaction.

``` python
def ui(self):
        '''Prompt a menu of functions, returning the selection entered by the user'''
        while True:
            print('------ Inventory Information Management System-------')
            print('| 1: Add stock')
            print('| 2: Remove stock')
            print('| 3: Get report')
            print('| 4: Query product information')
            print('| 0: Exit')
            print('------------------------------')
        
            choice = input('Please select from (0-4):')
            if choice == '0': return 'QUIT'
            elif choice == '1': return 'ADD_STOCK'
            elif choice == '2': return 'REMOVE_STOCK'
            elif choice == '3': return 'GET_REPORT'
            elif choice == '4': return 'QUERY_INFO'
```

Based on the interactive design, the system defines two functions: `prompt_for_name()` and `prompt_for_number()` with `input()` parameters for the user to input the specified stock type and its quantity.

``` python
    def prompt_for_name(self):
        '''Prompt the user for product name 'product' and return the product name, or return None'''
        while True:
                product = input("Input product name:")
                if product =="": 
                    return None
                else: 
                    return product

    def prompt_for_number(self):
        '''Prompt the user for product quantity 'number' and return the product quantity, or return None'''
        while True:
                number = input("Input quantity:")
                number = int(number)
                if number =="": 
                    return None
                else: 
                    return number
```
Planning the running sequence:
Combine basic functions and corresponding interactive functions, and control switching between different functions through conditional `if` and `elif` statements.

``` python
    def run(self):
        while True:
            action = self.ui()
            if action == 'QUIT':
                break

            elif action == 'ADD_STOCK':
                product = self.prompt_for_name()
                if product != None: 
                    number = self.prompt_for_number()
                    self.product = product
                    self.number = number
                    self.add_stock()

            elif action == 'REMOVE_STOCK':
                product = self.prompt_for_name()
                if product != None: 
                    number = self.prompt_for_number()
                    self.product = product
                    self.number = number
                    self.remove_stock()

            elif action == 'GET_REPORT':
                self.get_all_stock_status()

            elif action == 'QUERY_INFO':
                product = self.prompt_for_name()
                self.get_num_stock(product)
```
```

Implementation of instantiated objects: Instantiate the object from the class `Warehouse()` and call the method `Warehouse.run()` through the object.

``` python
if __name__ == "__main__":     
    obj = Warehouse()
    obj.run()
```




## Full Developed Code:

```python

class Stock:
    __stock_type = ["Kiwi", "Strawberry", "Papaya"] # create a list to record all the types that has been created
    def __init__(self, product: str, number: int) -> None:
        self.product = product
        self.number = number
        self.__stock_type.append(product)
                
    def stock_type(self) -> list:    
        return list(set(self.__stock_type))

class Warehouse(Stock):    
    __wh_id = []

    def __init__(self) -> None:
        super(Stock, self).__init__()
        self.id = self.generate_id()
        self.storage = {}
        print(f"Warehouse {self.id} is initiated.")

    def generate_id(self) -> int:
        '''Generate id for warehouse'''
        if not self.__wh_id:
            self.__wh_id.append(1)
            return self.__wh_id[-1]
        else:
            self.__wh_id.append(self.__wh_id[-1]+1)
            return self.__wh_id[-1]
    
    def update_stock_type(self) -> None:
        '''To make sure the warehouse storage dict is up to date'''
        for product in self.stock_type():
            if product not in self.storage.keys():
                self.storage[product] = 0

    def add_stock(self) -> None:
        '''Add an product and its number (quantity)'''
        assert self.number > 0, f"Only positive number allowed"
        self.update_stock_type()
        self.storage[self.product] += self.number

    def remove_stock(self) -> None:
        '''Remove stock by subtracting specified quantity of stock'''
        self.update_stock_type()       
        assert self.number > 0, f"Only positive number allowed"
        assert self.number <= self.storage[self.product], f"The storage is not enough"
        self.storage[self.product] -= self.number

    def get_all_stock_status(self) -> dict:
        '''Print the storage dict with Warehouse info'''
        self.update_stock_type()
        print("Warehouse ", self.id, self.storage)
        return self.storage

    def get_num_stock(self, name) -> int:
        ''' Query by the name '''
        self.update_stock_type()
        assert name in self.storage.keys(), "The item is not existed"
        print("The number of ", name, " in Warehouse ", self.id, " is ", self.storage[name])
        return self.storage[name]

    def ui(self):
        '''Prompt a menu of functions, returning the selection entered by the user'''
        while True:
            print('------ Inventory Information Management System-------')
            print('| 1: Add stock')
            print('| 2: Remove stock')
            print('| 3: Get report')
            print('| 4: Query product information')
            print('| 0: Exit')
            print('------------------------------')
        
            choice = input('Please select from (0-4):')
            if choice == '0': return 'QUIT'
            elif choice == '1': return 'ADD_STOCK'
            elif choice == '2': return 'REMOVE_STOCK'
            elif choice == '3': return 'GET_REPORT'
            elif choice == '4': return 'QUERY_INFO'


    def prompt_for_name(self):
        '''Prompt the user for product name 'product' and return the product name, or return None'''
        while True:
                product = input("Input product name:")
                if product =="": 
                    return None
                else: 
                    return product

    def prompt_for_number(self):
        '''Prompt the user for product quantity 'number' and return the product quantity, or return None'''
        while True:
                number = input("Input quantity:")
                number = int(number)
                if number =="": 
                    return None
                else: 
                    return number

    def run(self):
        while True:
            action = self.ui()
            if action == 'QUIT':
                break

            elif action == 'ADD_STOCK':
                product = self.prompt_for_name()
                if product != None: 
                    number = self.prompt_for_number()
                    self.product = product
                    self.number = number
                    self.add_stock()

            elif action == 'REMOVE_STOCK':
                product = self.prompt_for_name()
                if product != None: 
                    number = self.prompt_for_number()
                    self.product = product
                    self.number = number
                    self.remove_stock()

            elif action == 'GET_REPORT':
                self.get_all_stock_status()

            elif action == 'QUERY_INFO':
                product = self.prompt_for_name()
                self.get_num_stock(product)



if __name__ == "__main__":     
    obj = Warehouse()
    obj.run()
```

## Terminal Execution:

``` python
PS C:\Users\40695\Documents> & C:/Users/40695/AppData/Local/Microsoft/WindowsApps/python3.10.exe c:/Users/40695/Documents/Exercise/assi_v3.py
Warehouse 1 is initiated.
------ Inventory Information Management System-------
| 1: Add stock
| 2: Remove stock
| 3: Get report
| 4: Query product information
| 0: Exit
------------------------------
Please select from (0-4):3
Warehouse  1 {'Papaya': 0, 'Strawberry': 0, 'Kiwi': 0}
------ Inventory Information Management System-------
| 1: Add stock
| 2: Remove stock
| 3: Get report
| 4: Query product information
| 0: Exit
------------------------------
Please select from (0-4):1
Input product name:Kiwi
Input quantity:50
------ Inventory Information Management System-------
| 1: Add stock
| 2: Remove stock
| 3: Get report
| 4: Query product information
| 0: Exit
------------------------------
Please select from (0-4):3
Warehouse  1 {'Papaya': 0, 'Strawberry': 0, 'Kiwi': 50}
------ Inventory Information Management System-------
| 1: Add stock
| 2: Remove stock
| 3: Get report
| 4: Query product information
| 0: Exit
------------------------------
Please select from (0-4):2
Input product name:Kiwi
Input quantity:20
------ Inventory Information Management System-------
| 1: Add stock
| 2: Remove stock
| 3: Get report
| 4: Query product information
| 0: Exit
------------------------------
Please select from (0-4):3
Warehouse  1 {'Papaya': 0, 'Strawberry': 0, 'Kiwi': 30}
------ Inventory Information Management System-------
| 1: Add stock
| 2: Remove stock
| 3: Get report
| 4: Query product information
| 0: Exit
------------------------------
Please select from (0-4):4
Input product name:Kiwi
The number of  Kiwi  in Warehouse  1  is  30
------ Inventory Information Management System-------
| 1: Add stock
| 2: Remove stock
| 3: Get report
| 4: Query product information
| 0: Exit
------------------------------
Please select from (0-4):0
PS C:\Users\40695\Documents> 
```
Last Modified: 19/06/2022 by Peiyu Xiao