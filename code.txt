from datetime import datetime
import pyttsx3 as pt
import os
from twilio.rest import Client

def sendMessage(message):
    A_sid="ACa10a184e1e15e38c0458aea4fa6a58a4"
    A_token="d96b1984ded4d54e24bfe9314bddb0b9"
    client=Client(A_sid,A_token)
    m = client.messages.create(body=f"You have your {message}", from_='whatsapp:+14155238886', to='whatsapp:+917083636050')
    print(m)

engine=pt.init()
engine.setProperty('rate',125)

engine.setProperty('volume',1.0)
voices=engine.getProperty('voices')
engine.setProperty('voice',voices[0])


class ReminderSystem:
    def __init__(self):
        self.list = []


    def addReminder(self,l):
        self.list=l
        f=open('text.txt','a')
        f.write(f"{self.list[0]} {self.list[1]} {self.list[2]}\n")
        print("Reminder set successfully...")
        f.close()

    def activate(self):
        while True:
            f=open('text.txt')
            content = list(f.readlines())
            #print(content)
            #print(content)
            f.close()

            #print("in while")

            for i in range(0,len(content)):
                temp=list(content[i].strip().split())
                x = datetime.now()
                date = x.strftime("%x")
                date=date[3:6]+date[0:3]+date[6:]
                time = x.strftime("%X")
                time = time[:5]
                #print("time :",time)
                #print("date :", date)
                #print("temp[0] :", temp[0])
                #print("temp[1] :", temp[1])
                #print(temp)
               # print("in for")
                if date==temp[0] and time==temp[1]:
                    #call speak function
                    new = ""
                    for j in temp[2:]:
                        new = new + j + " "
                    print(f"You have your {new}")
                    self.speak(new)
                    self.deleteReminder(content[i])
                    return new

                else:
                    continue





    def deleteReminder(self,line):
        f = open('text.txt')
        content = f.readlines()
        f.close()
        content.remove(line)
        f = open('text.txt', 'w')
        f.writelines(content)
        f.close()



    def speak(self,s):
        engine.say(f"You have your {s}")
        engine.runAndWait()




if __name__=='__main__':
    ch=int(1)
    o=ReminderSystem()
    while(ch):
        print("<-----------Reminder System------------>")
        print(" 1. Add Reminder")
        print(" 2. Activate ")
        print("0. Exit")
        ch = int(input("Enter your choice : "))
        if ch==1:
            l=[input("Enter Date(dd/mm/yy): "), input("Enter Time(In 24 hours system) : "),input("Enter Event : ")]
            o.addReminder(l)

        elif ch==2:
            message = o.activate()
            sendMessage(message)











