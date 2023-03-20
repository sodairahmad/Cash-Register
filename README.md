# Cash-Register

#from customer import Customer
#from item import Item
#form invoice_item import InvoiceItem
from datetime import datetime


class Item:
    def __init__(self,id: int, name: str, price: float, measurment_unit:str) -> None:
        self.id = id 
        self.name = name
        self.price = price
        self.measurment_unit = measurment_unit
        
    def __repr__(self):
        return "< Class 'ITEMS'>"
    
    def __str__(self):
        return f"{self.name}: ${self.price}/{self.measurment_unit}"


class InvoiceItem:
    def __init__(self,item: Item, qty, discount: float = 0):
        self.item = item 
        self.qty = qty
        self.discount = discount
        
        #private property
        self._sub_total = (item.price * qty) - discount
    def __repr__(self):
        return "<Class 'InvoiceItem'>"
    
    def __str__(self):
        return (
            f"name is: {self.item.name} Quantity: {self.qty}, Discount: ${self.discount},"
            f"Sub total {self.get_sub_total():.2}" 
        )
    
    def get_sub_total(self):
        return self._sub_total





class Customer:
    def __init__(self,first_name,last_name):
        self.first_name = first_name
        self.last_name = last_name
        
    def __repr__(self):
        return "< class 'customer'>"
    
    def __str__(self) :
        return f"{self.first_name} {self.last_name}"

class CashRegister:
    def __init__(self, customer: Customer):
        self.customer= customer
        self.items: dict = {}
        self.purchase_date = datetime.now()
        self._invoice_total = 0
        
    def __repr__(self):
        return "< class 'Cashregister '>"
    
    def __str__(self):
        return f"Customer: {self.customer}, Total Items: {len(self.items)}"
    
    def _inc_invoice_total(self, item: InvoiceItem):
        self._invoice_total += item.get_sub_total()
        
    def _dec_invoice_total(self, item: InvoiceItem):
        self._invoice_total -= item.get_sub_total()
    
    def add_item(self, item: Item, qty: int= 1, discount: float=0):
        if item.name not in self.items:
            new_item = InvoiceItem(item,qty,discount)
            self.items[item.name] = new_item
            self._inc_invoice_total(new_item)
            
        else:
            print(f"{item.name} already in cart, update instead?")
    
    
    def update_item(self, item: Item, qty: int= 1, discount: float=0):
        if item.name in self.items:
            old_item = self.items[item.name]
            self._dec_invoice_total(old_item)
            
            new_item = InvoiceItem(item,qty,discount)
            self.items[item.name] = new_item
            self._inc_invoice_total(new_item)
            
    
            
    def remove_item(self, item: Item):
        if item.name in self.items:
            old_item = self.items[item.name]
            self._dec_invoice_total(old_item)
            
            del self.items[item.name]
            
    def get_invoice_total(self):
        return self._invoice_total
    
    def display_invoice(self):
        print()
        print("+" * 70)
        print(self)
        print(f"Date: {self.purchase_date.strftime('%B, %d, %Y')}") 
        print("-" * 70)
        for item in self.items.values():
            print(item)
        
        print("-" *70)  
        print(f"Total Price: ${self.get_invoice_total(): .2f}")
        print("+" *70)  
        
        
        
milk= Item(100,"milk",4.5,"litre")
egg = Item(101,"Egg", 0.99, "piece")
rice = Item(102, "Rice", 4, "Kg")
apple = Item(103, "Apple",5.67,"Kg")


Customer1= Customer("Louis", "Zaapa")
cr1 = CashRegister(Customer)
cr1.display_invoice()
cr1.add_item(milk)
cr1.add_item(rice,43,0.32)
print(Customer1)
    

cr1.display_invoice()

