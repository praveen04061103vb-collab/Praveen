# Praveen
A cybersecurity model for atm pin theft avoidances
import smtplib
from tkinter import *
import tkinter as tk
from tkinter import ttk, messagebox
import pymysql
import random
import mysql.connector as mysql

#---------------------------------------------------------------Login Function --------------------------------------
def clear():
        userid.delete(0,END)




def Transaction(uid):
        def amtdepo():
                con = mysql.connect(host="localhost",user="root",password="root",database="pinnumberdb")
                cur = con.cursor()

                cur.execute("select * from user_information where userid='%s' "%(uid))
                row = cur.fetchone()
                bal=int(lblentry.get()) + int(row[9])
                cur.execute(""" UPDATE user_information SET deposit='%s'  WHERE userid='%s' """% (bal,uid))
                con.commit()
                messagebox.showinfo("Deposited" , bal , parent = winTrans)

        def amtwith():
                con = mysql.connect(host="localhost",user="root",password="root",database="pinnumberdb")
                cur = con.cursor()

                cur.execute("select * from user_information where userid='%s' "%(uid))
                row = cur.fetchone()
                bal=int(row[9]) - int(lblentry1.get())
                cur.execute(""" UPDATE user_information SET deposit='%s'  WHERE userid='%s' """% (bal,uid))
                con.commit()
                messagebox.showinfo("Withdraw" , bal , parent = winTrans)

        def checkbalance():
                con = mysql.connect(host="localhost",user="root",password="root",database="pinnumberdb")
                cur = con.cursor()

                cur.execute("select * from user_information where userid='%s' "%(uid))
                row = cur.fetchone()
                messagebox.showinfo("Balance" , row[9] , parent = winTrans)


        def raise_frame(frame):
                frame.tkraise()

        winTrans = Tk()
        winTrans.title("Transaction Window")
        winTrans.maxsize(width=900, height=500)
        winTrans.minsize(width=900, height=500)

        # Set background color for the main window
        winTrans.configure(bg='lightblue')  # Set background color for the window

        # Create Frames
        second_frame = Frame(winTrans, bg='lightgreen')  # Set background color for the frame
        second_frame.place(x=0, y=0, width=900, height=500)

        third_frame = Frame(winTrans, bg='lightyellow')  # Set background color for the frame
        third_frame.place(x=0, y=0, width=900, height=500)

        first_frame = Frame(winTrans, bg='lightpink')  # Set background color for the frame
        first_frame.place(x=0, y=0, width=900, height=500)

        # Heading Label for the first frame
        Theading = Label(first_frame, text="MADE TRANSACTION HERE", font='Verdana 15 bold', bg='lightpink')
        Theading.place(x=50, y=50)

        # Buttons on the first frame
        btndepo = Button(first_frame, text="Deposit", font='Verdana 10 bold', command=lambda: raise_frame(second_frame))
        btndepo.place(x=100, y=150)

        btnwithdraw = Button(first_frame, text="Withdraw", font='Verdana 10 bold',
                             command=lambda: raise_frame(third_frame))
        btnwithdraw.place(x=200, y=150)

        btnBalance = Button(first_frame, text="Balance", font='Verdana 10 bold', command=checkbalance)
        btnBalance.place(x=300, y=150)

        btnExit = Button(first_frame, text="Exit", font='Verdana 10 bold', command=winTrans.quit)
        btnExit.place(x=400, y=150)

        # Second frame (Deposit screen)
        Theading = Label(second_frame, text="DEPOSIT", font='Verdana 15 bold', bg='lightgreen')
        Theading.place(x=50, y=50)

        lbldepo = Label(second_frame, text="Enter amount to Deposit:", font='Verdana 10 bold', bg='lightgreen')
        lbldepo.place(x=80, y=120)

        lblentry = Entry(second_frame, width=40)
        lblentry.place(x=300, y=120)

        btndepo = Button(second_frame, text="Deposit", font='Verdana 10 bold', command=amtdepo)
        btndepo.place(x=130, y=170)

        btnDExit = Button(second_frame, text="Exit", font='Verdana 10 bold', command=lambda: raise_frame(first_frame))
        btnDExit.place(x=250, y=170)

        # Third frame (Withdraw screen)
        Theading1 = Label(third_frame, text="Withdraw", font='Verdana 15 bold', bg='lightyellow')
        Theading1.place(x=50, y=50)

        lbldepo1 = Label(third_frame, text="Enter amount to withdraw:", font='Verdana 10 bold', bg='lightyellow')
        lbldepo1.place(x=80, y=120)

        lblentry1 = Entry(third_frame, width=40)
        lblentry1.place(x=300, y=120)

        btndepo1 = Button(third_frame, text="Withdraw", font='Verdana 10 bold', command=amtwith)
        btndepo1.place(x=130, y=170)

        btnDExit1 = Button(third_frame, text="Exit", font='Verdana 10 bold', command=lambda: raise_frame(first_frame))
        btnDExit1.place(x=250, y=170)

        # Raise the first frame by default
        raise_frame(first_frame)

        # Run the Tkinter event loop
        winTrans.mainloop()


def login():
        '''
        def switch(uid):
                winlogin.destroy()
                Transaction(uid)      
        '''

        def checklogin():

                if useridentry.get()=="":
                        messagebox.showerror("Error","Enter User id",parent=winlogin)
                else:
                        try:
                                con = mysql.connect(host="localhost",user="root",password="root",database="pinnumberdb")
                                cur = con.cursor()
                                cur.execute("select userid from user_information where userid = '%s' " % (
                                        useridentry.get()))
                                #cur.execute("select userid from user_information where userid=%s",(useridentry.get()))
                                row = cur.fetchone()


                                if row==None:
                                        messagebox.showerror("Error" , "Invalid User id", parent = winlogin)

                                else:
                                        messagebox.showinfo("Success" , "Successfully Login" , parent = winlogin)
                                        con.close()

                                        winlogin.destroy()
                                        dashboard(row[0])
                                        #switch(row[0])

                                        # close signup function


                        except Exception as es:
                                messagebox.showerror("Error" , f"Error Duo to : {str(es)}", parent = winlogin)

        winlogin = Tk()
        winlogin.title("Login Window")
        winlogin.maxsize(width=500, height=600)
        winlogin.minsize(width=500, height=600)

        # Set a colorful background color for the main window
        winlogin.configure(bg='#f0e68c')  # Light yellow background (feel free to change this)

        # Heading Label
        heading = Label(winlogin, text="Login", font='Verdana 20 bold', bg='#f0e68c',
                        fg='darkblue')  # Heading color is dark blue
        heading.place(x=80, y=60)

        # Form data label
        userid = Label(winlogin, text="User ID :", font='Verdana 10 bold', bg='#f0e68c', fg='darkblue')
        userid.place(x=80, y=130)

        # Entry Box for User ID
        userid_var = StringVar()
        useridentry = Entry(winlogin, width=40, textvariable=userid_var, font='Verdana 10')
        useridentry.focus()
        useridentry.place(x=170, y=130)

        # Button for login
        btn_login = Button(winlogin, text="Login", font='Verdana 10 bold', command=checklogin, bg='blue', fg='white')
        btn_login.place(x=200, y=200)

        # Button to clear the input
        btn_clear = Button(winlogin, text="Clear", font='Verdana 10 bold', command=clear, bg='gray', fg='white')
        btn_clear.place(x=300, y=200)

        # Signup button to switch to Signup page
        sign_up_btn = Button(winlogin, text="Switch To Sign up", font='Verdana 10 bold', command=signup, bg='green',
                             fg='white')
        sign_up_btn.place(x=320, y=20)

        # Start the Tkinter event loop
        winlogin.mainloop()

'''
#---------------------------------------------------------------End Login Function ---------------------------------
def withdrawwindow(uid):
        def action():
                con = pymysql.connect(host="localhost",user="root",password="",database="pinnumberdb")
                cur = con.cursor()
                
                cur.execute("select * from user_information where userid=%s ",uid)
                row = cur.fetchone()
                bal=int(row[9]) -int(lblentry.get())
                print("Balanceee is : " , bal)                          
        
        winDepo = Tk()
        winDepo.title("Withdraw Window")
        winDepo.maxsize(width=700 ,  height=300)
        winDepo.minsize(width=700 ,  height=300)
         #heading label
        Theading = Label(winDepo , text = "Withdraw" , font = 'Verdana 15 bold')
        Theading.place(x=50 , y=50)

        lbldepo = Label(winDepo, text= "Enter amount to Deposit :" , font='Verdana 10 bold')
        lbldepo.place(x=80,y=120)

        lblentry = Entry(winDepo, width=40)
        lblentry.place(x=300 , y=120)
        
        btndepo = Button(winDepo, text = "Deposit" ,font='Verdana 10 bold', command=lambda:action())
        btndepo.place(x=130, y=170)
        
        winDepo.mainloop()
'''

def depositwindow(uid):
        def action():
                con = mysql.connect(host="localhost",user="root",password="root",database="pinnumberdb")
                cur = con.cursor()

                cur.execute("select * from user_information where userid='%s' "%(uid))
                row = cur.fetchone()
                bal=int(lblentry.get() +int(row[9])  )
                print("Balanceee is : " , bal)

        winDepo = Tk()
        winDepo.title("Deposit Window")
        winDepo.maxsize(width=700, height=300)
        winDepo.minsize(width=700, height=300)

        # Set background color for the window
        winDepo.configure(bg='#e6f7ff')  # Light blue background

        # Heading label
        Theading = Label(winDepo, text="DEPOSIT", font='Verdana 15 bold', bg='#e6f7ff', fg='darkblue')
        Theading.place(x=50, y=50)

        # Label for deposit amount
        lbldepo = Label(winDepo, text="Enter amount to Deposit:", font='Verdana 10 bold', bg='#e6f7ff', fg='darkblue')
        lbldepo.place(x=80, y=120)

        # Entry field for the deposit amount
        lblentry = Entry(winDepo, width=40)
        lblentry.place(x=300, y=120)

        # Deposit button
        btndepo = Button(winDepo, text="Deposit", font='Verdana 10 bold', command=action, bg='blue', fg='white')
        btndepo.place(x=130, y=170)

        # Start the Tkinter event loop
        winDepo.mainloop()



#---------------------------------------------------- DashBoard Panel -----------------------------------------
def dashboard(uid):

        def desclose():
                des.destroy()


        def redclick():
                if text2.get()=="":
                        text2.delete(0, tk.END)
                        text2.insert(0, "R")
                else:
                     text2.insert(tk.END, "R")

        def blueclick():
                if text2.get()=="":
                        text2.delete(0, tk.END)
                        text2.insert(0, "B")
                else:
                     text2.insert(tk.END, "B")

        def greenclick():
                if text2.get()=="":
                        text2.delete(0, tk.END)
                        text2.insert(0, "G")
                else:
                     text2.insert(tk.END, "G")

        def checkcount1():
                con = mysql.connect(host="localhost",user="root",password="root",database="pinnumberdb")
                cur = con.cursor()
                print(uid)
                text3 = Entry(des, width=20, text=" ")
                bal = int(text3.get())
                cur.execute(""" UPDATE user_information SET pinnumber='%s'  WHERE userid='%s' """ % (bal, uid))





                con.commit()
                s1="Pin Changed Successfully"
                messagebox.showinfo("Success",s1)
        def checkcount():
                text = text2.get()
                c = len(text)

                if c >3 or c<3:
                        print(c)
                        text2.delete(0, tk.END)
                        messagebox.showerror("Invalid" , "YOUR PIN ENTRY INVALID" , parent = des)
                elif text=="RBG":
                        pno.insert(tk.END,"1")
                        text2.delete(0, tk.END)
                elif text=="RGG":
                        pno.insert(tk.END,"2")
                        text2.delete(0, tk.END)
                elif text=="GGG":
                        pno.insert(tk.END,"3")
                        text2.delete(0, tk.END)
                elif text=="GRR":
                        pno.insert(tk.END,"4")
                        text2.delete(0, tk.END)
                elif text=="GRG":
                        pno.insert(tk.END,"5")
                        text2.delete(0, tk.END)

                elif text=="GRB":
                        pno.insert(tk.END,"6")
                        text2.delete(0, tk.END)

                elif text=="BRB":
                        pno.insert(tk.END,"7")
                        text2.delete(0, tk.END)

                elif text=="RRR":
                        pno.insert(tk.END,"8")
                        text2.delete(0, tk.END)

                elif text=="GBB":
                        pno.insert(tk.END,"9")
                        text2.delete(0, tk.END)

                elif text=="GBR":
                        pno.insert(tk.END,"0")
                        text2.delete(0, tk.END)

                else:
                        text2.delete(0, tk.END)
                        messagebox.showerror("Invalid" , "YOUR PIN ENTRY MISMATCHED" , parent = des)

        def verifypin():
                p = pno.get()

                cur.execute("select pinnumber from user_information where userid='%s' "%(uid))
                row = cur.fetchone()
                if row[0] == p :
                        messagebox.showinfo("Valid" , "YOUR PIN ENTRY IS MATCHED" , parent = des)
                        desclose()
                        Transaction(uid)
                else:
                        messagebox.showerror("Invalid" , "YOUR PIN ENTRY IS MISMATCHED" , parent = des)


        des = Tk()
        des.title("Admin Panel ")
        des.maxsize(width=800 ,  height=500)
        des.minsize(width=800 ,  height=500)


        con = mysql.connect(host="localhost",user="root",password="root",database="pinnumberdb")
        cur = con.cursor()

        cur.execute("select username from user_information where userid='%s' "%(uid))
        row = cur.fetchone()

        heading = Label(des , text = "Welcome " + row[0] , font = 'Verdana 10 bold',bg='yellow')
        heading.place(x=300 , y=5)

        pno = Entry(des, width=45)
        pno.place(x=200 , y=25)

        btn_verify = Button(des, text = "Verify" ,font='Verdana 10 bold' , command = verifypin )
        btn_verify.place(x=500, y=25)

        imagetest0 = PhotoImage(file="0.png")
        btn0 = Button(des,    image=imagetest0)
        btn0.place(x=200, y=50)

        imagetest1 = PhotoImage(file="1.png")
        btn1 = Button(des,     image=imagetest1)
        btn1.place(x=300, y=50)

        imagetest2 = PhotoImage(file="2.png")
        btn1 = Button(des,     image=imagetest2)
        btn1.place(x=400, y=50)

        imagetest3 = PhotoImage(file="3.png")
        btn3 = Button(des,    image=imagetest3)
        btn3.place(x=200, y=150)

        imagetest4 = PhotoImage(file="4.png")
        btn4 = Button(des,     image=imagetest4)
        btn4.place(x=300, y=150)

        imagetest5 = PhotoImage(file="5.png")
        btn5 = Button(des,     image=imagetest5)
        btn5.place(x=400, y=150)

        imagetest6 = PhotoImage(file="6.png")
        btn6 = Button(des,    image=imagetest6)
        btn6.place(x=200, y=250)

        imagetest7 = PhotoImage(file="7.png")
        btn7 = Button(des,     image=imagetest7)
        btn7.place(x=300, y=250)

        imagetest8 = PhotoImage(file="8.png")
        btn8 = Button(des,     image=imagetest8)
        btn8.place(x=400, y=250)

        imagetest9 = PhotoImage(file="9.png")
        btn9 = Button(des,     image=imagetest9)
        btn9.place(x=200, y=350)

        imagetest10 = PhotoImage(file="can.png")
        btn_can = Button(des,     image=imagetest10)
        btn_can.place(x=300, y=350)

        imagetest11 = PhotoImage(file="ref.png")
        btn_ref = Button(des,     image=imagetest11)
        btn_ref.place(x=400, y=350)

        text2 = Entry(des, width=20 , text=" " ,show='*')
        text2.place(x=500 , y=220)

        btn_red = Button(des, width=6,height=3, bg='red', command=lambda:redclick() )
        btn_red.place(x=500, y=250)

        btn_green = Button(des,width=6,height=3, bg='green' , command=lambda:greenclick()    )
        btn_green.place(x=570, y=250)

        btn_blue = Button(des,width=6,height=3,  bg='blue', command=lambda:blueclick()     )
        btn_blue.place(x=640, y=250)

        btn_process = Button(des, text="Process",font='Verdana 10 bold', command = checkcount )
        btn_process.place(x=500, y=350)
        text3 = Entry(des, width=20, text=" ")
        text3.place(x=600, y=350)
        btn_process1 = Button(des, text="Pin Change", font='Verdana 10 bold', command=checkcount1)
        btn_process1.place(x=600, y=400)


        des.mainloop()

#-----------------------------------------------------End Deshboard Panel -------------------------------------
#----------------------------------------------------------- Signup Window --------------------------------------------------
def mainclose():
                win.destroy()

def signup():

        mainclose()

        # signup database connect
        def action():
                if userid.get()=="" or address.get()=="" or emailid.get()=="" or mobile.get()=="" or pinnumber.get()=="" or accounttype.get()=="" or accountnumber.get()=="" or branchname.get()=="" or deposit.get()=="" or nomination.get()=="":
                        messagebox.showerror("Error" , "All Fields Are Required" , parent = winsignup)

                else:

                        try:

                                con = mysql.connect(host="localhost",user="root",password="root",database="pinnumberdb")
                                cur = con.cursor()
                                cur.execute("select * from user_information where username='%s'"%(username.get()))
                                row = cur.fetchone()
                                if row!=None:
                                        messagebox.showerror("Error" , "User Name Already Exits", parent = winsignup)
                                else:

                                        cur.execute("insert into user_information(username,address,emailid,mobile,pinnumber,accounttype,accountnumber,branchname,deposit,nomination) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)",
                                                (
                                                username.get(),
                                                address.get(),
                                                emailid.get(),
                                                mobile.get(),
                                                pinnumber.get(),
                                                accounttype.get(),
                                                accountnumber.get(),
                                                branchname.get(),
                                                deposit.get(),
                                                nomination.get()
                                                ))
                                        con.commit()
                                        con.close()
                                        str1 = " Hi user this is your ATM Pin Number !!!" + pinnumber.get()

                                        server = smtplib.SMTP_SSL("smtp.gmail.com", 465)
                                        email_addr = 'sarun140689@gmail.com'
                                        email_passwd = 'almqpdhzxzerjmqj'
                                        server.login(email_addr, email_passwd)
                                        server.sendmail(from_addr=email_addr, to_addrs=emailid.get(),
                                                        msg=str1)

                                messagebox.showinfo("Success" , "Registered Successfully" , parent = winsignup)
                                clear()


                        except Exception as es:
                                messagebox.showerror("Error" , f"Error Due to : {str(es)}", parent = winsignup)

        # close signup function
        def switch():
                winsignup.destroy()
                login()

        # clear data function
        def clear():
                userid.delete(0,END)
                username.delete(0,END)
                address.delete(0,END)
                emailid.delete(0,END)
                mobile.delete(0,END)
                pinnumber.delete(0,END)
                accounttype.delete(0,END)
                accountnumber.delete(0,END)
                branchname.delete(0,END)
                deposit.delete(0,END)
                nomination.delete(0,END)
        def generatepin():
                global pinno
                pinno = str(random.randint(1000,10000))
                print(pinno)
                input_text.set(pinno)


        # start Signup Window

        winsignup = tk.Tk()
        winsignup.title("Signup Window")
        winsignup.maxsize(width=800, height=600)
        winsignup.minsize(width=800, height=600)

        # Set background color for the window
        winsignup.config(bg="lightblue")


        #heading label
        heading = Label(winsignup , text = "Signup" , font = 'Verdana 20 bold')
        heading.place(x=80 , y=60)

        # form data label
        userid = Label(winsignup, text= "User ID :" , font='Verdana 10 bold')
        userid.place(x=80,y=130)

        username = Label(winsignup, text= "User Name :" , font='Verdana 10 bold')
        username.place(x=80,y=160)

        address = Label(winsignup, text= "Address :" , font='Verdana 10 bold')
        address.place(x=80,y=190)


        emailid = Label(winsignup, text= "Email ID :" , font='Verdana 10 bold')
        emailid.place(x=80,y=260)

        mobile = Label(winsignup, text= "Mobile :" , font='Verdana 10 bold')
        mobile.place(x=80,y=290)

        pinnumber = Label(winsignup, text= "Pin Number :" , font='Verdana 10 bold')
        pinnumber.place(x=80,y=320)

        accounttype = Label(winsignup, text= "Account Type :" , font='Verdana 10 bold')
        accounttype.place(x=80,y=350)

        accountnumber = Label(winsignup, text= "Account No :" , font='Verdana 10 bold')
        accountnumber.place(x=80,y=380)

        branchname = Label(winsignup, text= "Branch Name :" , font='Verdana 10 bold')
        branchname.place(x=80,y=410)

        deposit = Label(winsignup, text= "Deposit :" , font='Verdana 10 bold')
        deposit.place(x=80,y=440)

        nomination = Label(winsignup, text= "Nomination :" , font='Verdana 10 bold')
        nomination.place(x=80,y=470)

        # Entry Box ------------------------------------------------------------------

        uid = StringVar()
        username = StringVar()
        address = StringVar()
        emailid= StringVar()
        mobile= StringVar()
        pinno=StringVar()
        accounttype = StringVar()
        accountnumber=StringVar()
        branchname = StringVar()
        deposit = StringVar()
        nomination=StringVar()

        uid = ""
        print(uid)
        pintext = StringVar()
        pintext.set(uid)

        userid = Entry(winsignup, width=40, text = ""  )
        userid.place(x=200 , y=133)

        con = mysql.connect(host="localhost",user="root",password="root",database="pinnumberdb")
        cur = con.cursor()

        cur.execute("select MAX(userid) from user_information")
        row = cur.fetchone()
        uid = row[0]
        if uid == None:
                uid=0
        uid=uid+1
        userid.delete(0, tk.END)
        userid.insert(0, uid)

        username = Entry(winsignup, width=40 , textvariable = username)
        username.focus()
        username.place(x=200 , y=163)


        address = Entry(winsignup, width=40, textvariable=address)
        address.place(x=200 , y=193)


        emailid = Entry(winsignup, width=40,textvariable = emailid)
        emailid.place(x=200 , y=263)

        mobile = Entry(winsignup, width=40 , textvariable = mobile)
        mobile.place(x=200 , y=293)


        Genpin = Button(winsignup, text = "Generate Pin" ,font='Verdana 10 bold', command = lambda: generatepin())
        Genpin.place(x=470,y=323)

        pinno = ""
        input_text = StringVar()

        pinnumber = Entry(winsignup, width=40, text="",  textvariable = input_text)
        pinnumber.place(x=200 , y=323)

        accounttype = Entry(winsignup, width=40,textvariable = accounttype)
        accounttype.place(x=200 , y=353)


        accountnumber = Entry(winsignup, width=40, textvariable = accountnumber)
        accountnumber.place(x=200 , y=383)

        branchname = Entry(winsignup, width=40, textvariable = branchname)
        branchname.place(x=200 , y=413)

        deposit = Entry(winsignup, width=40, textvariable = deposit)
        deposit.place(x=200 , y=443)

        nomination = Entry(winsignup, width=40, textvariable = nomination)
        nomination.place(x=200 , y=473)

        # button login and clear

        btn_signup = Button(winsignup, text = "Signup" ,font='Verdana 10 bold', command = action)
        btn_signup.place(x=200, y=500)


        btn_login = Button(winsignup, text = "Clear" ,font='Verdana 10 bold' , command = clear)
        btn_login.place(x=280, y=500)


        sign_up_btn = Button(winsignup , text="Switch To Login" , command = switch )
        sign_up_btn.place(x=350 , y =20)


        winsignup.mainloop()
#---------------------------------------------------------------------------End Singup Window-----------------------------------

#------------------------------------------------------------ Login Window -----------------------------------------

from tkinter import Tk, Button, Canvas, PhotoImage

# Creating the main window
win = Tk()

# Set the app title
win.title("Pin Number App")

# Set the window size
win.maxsize(width=800, height=800)
win.minsize(width=800, height=800)

# Set background color for the window
win.config(bg="#ADD8E6")

# Create a canvas to display an image (make sure the image path is correct)
canvas = Canvas(win, width=300, height=300, bg="#ADD8E6", bd=0, highlightthickness=0)
canvas.pack(pady=130)  # Adds some padding to position the image better
img = PhotoImage(file="pinimg.png")  # Update the image path
canvas.create_image(100, 150, anchor="center", image=img)  # Center the image
Theading = Label(win, text="ATM PIN ENTRY SYSTEM", font="Verdana 12 bold", fg="white", bg="#4CAF50",
                   relief="raised", width=20, height=2)
Theading.place(x=230, y=50)
# Create "Admin" button with modern styling
btn_admin = Button(win, text="Authentication", font="Verdana 12 bold", fg="white", bg="#24a3d1",
                   relief="raised", width=20, height=2, command = signup)
btn_admin.place(x=340, y=500, anchor="center")  # Center the button horizontally



# Run the Tkinter event loop
win.mainloop()

#-------------------------------------------------------------------------- End Login Window ---------------------------------------------------
'''
create database pinnumberdb
 
CREATE TABLE user_information(
        userid int primary key auto_increment NOT NULL,
        username varchar(50) NULL,
        address varchar(50) NULL,
        emailid varchar(50) NULL, 
        mobile varchar(50) NULL,
        pinnumber varchar(250) NULL,
        accounttype varchar(250) NULL,
        accountnumber varchar(250) NULL,
        branchname varchar(250) NULL,
        deposit varchar(250) NULL,
        nomination varchar(250) NULL)
        
'''
