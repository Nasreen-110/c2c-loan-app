# c2c-loan-app
import pickle
import os
import pathlib
from tkinter import *
class LoanCalculator:
    def __init__(self):
        window = Tk ()
        window.title ("Loan Calculator")
        Label (window, text="Annual Interest Rate").grid (row=1,
                                                          column=1, sticky=W)
        Label (window, text="Number of Years").grid (row=2,
                                                     column=1, sticky=W)
        Label (window, text="Loan Amount").grid (row=3,
                                                 column=1, sticky=W)
        Label (window, text="Monthly Payment").grid (row=4,
                                                     column=1, sticky=W)
        Label (window, text="Total Payment").grid (row=5,
                                                   column=1, sticky=W)
        self.annualInterestRateVar = StringVar ()
        Entry (window, textvariable=self.annualInterestRateVar,
               justify=RIGHT).grid (row=1, column=2)
        self.numberOfYearsVar = StringVar ()
        Entry (window, textvariable=self.numberOfYearsVar,
               justify=RIGHT).grid (row=2, column=2)
        self.loanAmountVar = StringVar ()
        Entry (window, textvariable=self.loanAmountVar,
               justify=RIGHT).grid (row=3, column=2)
        self.monthlyPaymentVar = StringVar ()
        lblMonthlyPayment = Label (window, textvariable=
        self.monthlyPaymentVar).grid (row=4,
                                      column=2, sticky=E)
        self.totalPaymentVar = StringVar ()
        lblTotalPayment = Label (window, textvariable=
        self.totalPaymentVar).grid (row=5,
                                    column=2, sticky=E)
        btComputePayment = Button (window, text="Compute Payment",
                                   command=self.computePayment).grid (
            row=6, column=2, sticky=E)
        window.mainloop ()
    def computePayment(self):
        monthlyPayment = self.getMonthlyPayment (
            float (self.loanAmountVar.get ()),
            float (self.annualInterestRateVar.get ()) / 1200,
            int (self.numberOfYearsVar.get ()))
        self.monthlyPaymentVar.set (format (monthlyPayment, '10.2f'))
        totalPayment = float (self.monthlyPaymentVar.get ()) * 12 \
                       * int (self.numberOfYearsVar.get ())
        self.totalPaymentVar.set (format (totalPayment, '10.2f'))
    def getMonthlyPayment(self, loanAmount, monthlyInterestRate, numberOfYears):
        monthlyPayment = loanAmount * monthlyInterestRate / (1 - 1 /
                                                             (1 + monthlyInterestRate) ** (numberOfYears * 12))
        return monthlyPayment;
        root = Tk ()
class Account :
   accNo = 0
   name = "deposit=0 type = "
   def createAccount(self):
       self.accNo= int(input("Enter the account no : "))
       self.name = input("Enter the account holder name : ")
       self.type = input("Ente the type of account [C/S] : ")
       self.deposit = int(input("Enter The Initial amount(>=500 for Saving and >=1000 for current: "))
       print("\n\nAccount Created")
   def showAccount(self):
       print("Account Number : ",self.accNo)
       print("Account Holder Name : ", self.name)
       print("Type of Account",self.type)
       print("Balance : ",self.deposit)
   def modifyAccount(self):
       print("Account Number : ",self.accNo)
       self.name = input("Modify Account Holder Name :")
       self.type = input("Modify type of Account :")
       self.deposit = int(input("Modify Balance :"))
   def depositAmount(self,amount):
       self.deposit += amount
   def withdrawAmount(self,amount):
       self.deposit -= amount
   def report(self):
       print(self.accNo, " ",self.name ," ",self.type," ", self.deposit)
   def getAccountNo(self):
       return self.accNo
   def getAcccountHolderName(self):
       return self.name
   def getAccountType(self):
       return self.type
   def getDeposit(self):
       return self.deposit
def intro():
   print("\t\t\t\t**********************")
   print("\t\t\t\tC2C ")
   print("\t\t\t\t**********************")
   print("\t\t\t\tPersonal Loan Lending")
   print("\t\t\t\tMobile Application")
def writeAccount():
   account = Account()
   account.createAccount()
   writeAccountsFile(account)
def displayAll():
   file = pathlib.Path("accounts.data")
   if file.exists ():
       infile = open('accounts.data','rb')
       mylist = pickle.load(infile)
       for item in mylist :
           print(item.accNo," ", item.name, " ",item.type, " ",item.deposit )
       infile.close()
   else :
       print("No records to display")
def displaySp(num):
   file = pathlib.Path("accounts.data")
   if file.exists ():
       infile = open('accounts.data','rb')
       mylist = pickle.load(infile)
       infile.close()
       found = False
       for item in mylist :
           if item.accNo == num :
               print("Your account Balance is = ",item.deposit)
               found = True
   else :
       print("No records to Search")
   if not found :
       print("No existing record with this number")
def depositAndWithdraw(num1,num2):
   file = pathlib.Path("accounts.data")
   if file.exists ():
       infile = open('accounts.data','rb')
       mylist = pickle.load(infile)
       infile.close()
       os.remove('accounts.data')
       for item in mylist :
           if item.accNo == num1 :
               if num2 == 1 :
                   amount = int(input("Enter the amount to deposit : "))
                   item.deposit += amount
                   print("Your account is updted")
               elif num2 == 2 :
                   amount = int(input("Enter the amount to withdraw : "))
                   if amount <= item.deposit :
                       item.deposit -=amount
                   else :
                       print("You cannot withdraw larger amount")
   else :
       print("No records to Search")
   outfile = open('newaccounts.data','wb')
   pickle.dump(mylist, outfile)
   outfile.close()
   os.rename('newaccounts.data', 'accounts.data')
def deleteAccount(num):
   file = pathlib.Path("accounts.data")
   if file.exists ():
       infile = open('accounts.data','rb')
       oldlist = pickle.load(infile)
       infile.close()
       newlist = []
       for item in oldlist :
           if item.accNo != num :
               newlist.append(item)
       os.remove('accounts.data')
       outfile = open('newaccounts.data','wb')
       pickle.dump(newlist, outfile)
       outfile.close()
       os.rename('newaccounts.data', 'accounts.data')
def modifyAccount(num):
   file = pathlib.Path("accounts.data")
   if file.exists ():
       infile = open('accounts.data','rb')
       oldlist = pickle.load(infile)
       infile.close()
       os.remove('accounts.data')
       for item in oldlist :
           if item.accNo == num :
               item.name = input("Enter the account holder name : ")
               item.type = input("Enter the account Type : ")
               item.deposit = int(input("Enter the Amount : "))
       outfile = open('newaccounts.data','wb')
       pickle.dump(oldlist, outfile)
       outfile.close()
       os.rename('newaccounts.data', 'accounts.data')
def writeAccountsFile(account) :
   file = pathlib.Path("accounts.data")
   if file.exists ():
       infile = open('accounts.data','rb')
       oldlist = pickle.load(infile)
       oldlist.append(account)
       infile.close()
       os.remove('accounts.data')
   else :
       oldlist = [account]
   outfile = open('newaccounts.data','wb')
   pickle.dump(oldlist, outfile)
   outfile.close()
   os.rename('newaccounts.data', 'accounts.data')
# start of the program
ch="num=0 "
intro()
while ch != 9:
   #system("cls");
   print("\tMAIN MENU")
   print("\t1. NEW ACCOUNT")
   print("\t2. DEPOSIT AMOUNT")
   print("\t3. WITHDRAW AMOUNT")
   print("\t4. BALANCE ENQUIRY")
   print("\t5. ALL ACCOUNT HOLDER LIST")
   print("\t6. CLOSE AN ACCOUNT")
   print("\t7. MODIFY AN ACCOUNT")
   print("\t8. Calculator")
   print("\t9. EXIT")
   print("\tSelect Your Option (1-9) ")
   ch = input ("Enter your choice : ")
   if ch == '1':
       writeAccount()
   elif ch == '2':
       num = int(input("\tEnter The account No. : "))
       depositAndWithdraw(num, 1)
   elif ch == '3':
       num = int(input("\tEnter The account No. : "))
       depositAndWithdraw(num, 2)
   elif ch == '4':
       num = int(input("\tEnter The account No. : "))
       displaySp(num)
   elif ch == '5':
       displayAll();
   elif ch == '6':
       num =int(input("\tEnter The account No. : "))
       deleteAccount(num)
   elif ch == '7':
       num = int(input("\tEnter The account No. : "))
       modifyAccount(num)
   elif ch == '8':
       LoanCalculator ()
   elif ch == '9':
       print("\tThanks for using bank management system")
       break
   else :
       print("Invalid choice")
