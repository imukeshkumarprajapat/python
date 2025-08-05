import json
import random
import string
from pathlib import Path  


class Bank:

    #dummy data
    database="BANK_MANAGEMENT\data.json"
    #location nhi mile to ye dalo ho mil jayegi database = str(Path(__file__).parent / "data.json")
    data=[]
    
    try:
        if Path(database).exists():
         with open(database)as fs:
          data =json.loads(fs.read())
        else:
           print("no such file exist")
           
    except Exception as err:
        print(f"an exception occured as {err}")
    
    @classmethod
    def __update(cls):
       with open(cls.database,'w') as fs:
          fs.write(json.dumps(Bank.data))
    
    @classmethod
    def __accountgenerate(cls):
       alpha=random.choices(string.ascii_letters,k=3)
       num=random.choices(string.digits, k=3)
       spchar=random.choices("!@#$%^&*",k=1)
       id=alpha+num+spchar
       random.shuffle(id)
       return "".join(id)

    def Createaccount(self):
        info={
           "name":input("Tell Your Name:-"),
           "age":int(input("Tell Your Age:-")),
           "Email":input("Tell Your Email:-"),
           "pin":int(input("Tell Your 4 pin:-")),
           "AccountNo.":Bank.__accountgenerate(),
           "balance":0
        }
        if info['age']< 18 or len(str(info['pin']))!=4:
           print("Sorry you cannot create your account")
        else:
           print("Account has been created successfully")
           
           for i in info:
               print(f"{i}:{info[i]}")
           print("Please note down your account number")
            
           Bank.data.append(info)
           
           Bank.__update()
    
    def depositmoney(self):
       accnumber=input("Please tell your account number")
       pin= int(input("Please tell your pin aswell"))

       userdata=[i for i in Bank.data if i["AccountNo."] == accnumber and i['pin']==pin]

       if userdata==False:
          print("Sorry no data found")

       else:
            amount=int(input("HOw much you want to deposit"))
            
            if amount> 10000 or amount< 0:
               print("sorry the amount is too much you can deposit below 10000 and above 0")
            
            else:
               
               userdata[0]['balance']+=amount
               Bank.__update()
               print("Amount deposited successfully")

    def withdrawmoney(self):
       accnumber=input("Please tell your account number")
       pin= int(input("Please tell your pin aswell"))

       userdata=[i for i in Bank.data if i["AccountNo."]==accnumber and i['pin']==pin]

       if userdata==False:
          print("Sorry no data found")
       else:
            amount=int(input("HOw much you want to withdraw money"))
            if userdata[0]['balance']<amount:
               print("sorry you dont have that much money")
            
            else:
               
               userdata[0]['balance']-=amount
               Bank.__update()
               print("Amount withdrew successfully")

    def showdetails(self):
       accnumber=input("Please tell your account number")
       pin= int(input("Please tell your pin aswell"))
       userdata=[i for i in Bank.data if i["AccountNo."] == accnumber and i['pin']==pin]
       print("print Your information are \n \n\n")
       for i in userdata[0]:
          print(f"{i}:{userdata[0][i]}")

    def updatedetails(self):
       accnumber=input("Please tell your account number")
       pin=int(input("Please tell your pin aswell"))
       userdata=[i for i in Bank.data if i["AccountNo."]==accnumber and i['pin']==pin]
       
       if userdata==False:
          print("No such user found")

       else:
          print("You cannot change the age, account number, balance")

          print("fill the details for change or leave it empty if no change")

          newdata={
             "name":input("Please tell new name or press enter:"),
             "Email":input("Please tell your new Email or press enter to skip:"),
             "pin":input("Enter your new pin or press enter to skip:")
          }
          
          if newdata["name"]=="":
             newdata["name"]=userdata[0]['name']

          if newdata["Email"]=="":
             newdata["Email"]=userdata[0]['Email']

          if newdata["pin"]=="":
             newdata["pin"]=userdata[0]['pin']
          
          newdata['age']=userdata[0]['age']
          newdata['AccountNo.']=userdata[0]['AccountNo.']
          newdata['balance']=userdata[0]['balance']

          if type(newdata['pin'])==str:
             newdata["pin"]=int(newdata["pin"])

          for i in newdata:
             if newdata[i]==userdata[0][i]:
                continue
             else:
                userdata[0][i]=newdata[i]

          Bank.__update()
          print("Details updates successfully")


    def delete(self):
       accnumber=input("Please tell your account number")
       pin=int(input("Please tell your pin aswell"))
       userdata=[i for i in Bank.data if i["AccountNo."]==accnumber and i['pin']==pin]

       if userdata==False:
          print("Sorry no such data exist")
       else:
          check= input("press y if you actually want to do delete the account or press n")
          if check=='n' or check== 'N':
             print("bypassed")
          else:
             index=Bank.data.index(userdata[0])
             Bank.data.pop(index)
             print("account deleted successfully")
             Bank.__update()
          
               
user=Bank()
print("Press 1 forcreating an account")
print("press 2 for Depositing the money in the bank")
print("press 3 for withdrawing the money")
print("press 4 for details")
print("press 5 for updating the details ")
print("press 6 for deleting your account")


check=int(input("Tell your response:-"))

if check == 1:
    user.Createaccount()

if check==2:
    user.depositmoney() 

if check==3:
    user.withdrawmoney()

if check==4:
    user.showdetails()

if check==5:
   user.updatedetails()

if check==6:
   user.delete()
