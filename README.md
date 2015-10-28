# Project_Pandian
24. The G.L RESORTS is a Private limited Company which has a chain of resorts all over the state. It has a range of luxurious as well as economic rooms &amp; suites for the customers to stay in. When a Customers wants to  book rooms , he/she can view the room details and customer has to pay in  advance  10%of the total payment has to be made based on the room rent.  A booking request can be accepted only when vacancy is there according to the requirement. During check-out the total bill amount has to be cleared by the customer. Total bill amount should include the room rent, and 15% service tax of the total bill amount .A customer can book a room over phone and if within 5 hours of booking the customer does not checks in, booking is canceled and 2% from the advance payment is deducted. The company requests  you  to develop  a complete system to bring  automation for its room booking process.


 HMS2_cu.hpp
 
#ifndef HMS2_cu.hpp
#define HMS2_cu.hpp
class Customer
{
  public:
        bool connection();
        int login(char[],char[]);
        void registration(char[],char[],char[],double,char[],char[]);

        void book(int);
        void viewroomdetails();


};
#endif
***********************************************************************************
HMS2.pc


#include"HMS2_cu.hpp"
#include<iostream>
#include<cstring>
#include<string.h>
#include<sqlda.h>
#include<sqlcpr.h>
#define NODATAFOUND 1403
using namespace std;
EXEC SQL INCLUDE SQLCA.H;

EXEC SQL BEGIN DECLARE SECTION;
char userId[20];
char pwd[20];
char x[20],a[30],b[30],i[20],j[20],k[20],l[20],p[30];
char y[20],m[30];
char d_uname[20];
char d_room_type[10],d_available[30];
int d_room_no,d_tariff,cmp1,cmp2,f=0;
char d_password[20];
char c_name[20];
int ch,ch2=1,cal;
char c_city[10];
double c_phone;
char dummy1[20],dummy2[20];
char gender[10];
EXEC SQL END DECLARE SECTION;

bool Customer::connection()
{
 strcpy(d_uname,"E1038416@UnixDB03");
 strcpy(d_password,"E1038416");
 EXEC SQL CONNECT :d_uname IDENTIFIED BY :d_password;
 if(sqlca.sqlcode != 0)
 return false;
 else
 return true;
}



int Customer::login(char user[20],char pass[20])
{
 if(connection())
 {
    strcpy(d_uname,user);
    strcpy(d_password,pass);




        EXEC SQL DECLARE Curs CURSOR FOR select uname,password  from cust_details;
        EXEC SQL OPEN Curs;
        for(;;)
        {
          EXEC SQL FETCH Curs INTO :a,:b;
          if(sqlca.sqlcode == NODATAFOUND)
          {
           break;
          }
          if(sqlca.sqlcode<0)
          {
          cout<<"ERROR";
          }
          else
          {
            cmp1=strcmp(a,d_uname);
            //cout<<a<<b<<d_uname<<d_password;
            cmp2=strcmp(b,d_password);
            if(cmp1==cmp2)
            {
                f=1;

            }
            else
            {
             f=0;
            }
         }
      }
  if(f==1)
 {
  cout<<"Login Successfull\n";
    return 0;/*
do
{
  cout<<"Welcome Customer\n";
  cout<<"Press\n"<<"1. To view Rooms\n"<<"2.To Book a Room\n"<<"3.To exit\n";
  cin>>ch;
  switch(ch)
     {
      case 1:
                cout<<"\nThese are available rooms";
                viewroomdetails();
                //cout<<"Please choose your room";
                break;
      case 2:
                cout<<"\nPlease enter your room no";
                cin>>d_room_no;

                book(d_room_no);
        default:
                ch2=0;
                break;
     }

}while(ch2==1);
*/ }
else
  {
  cout<<"not successful\n";
  return 1;
  }
  EXEC SQL COMMIT WORK RELEASE;
 }
 else
 {
        cout<<"Connection failed";
       }

}

void Customer::registration(char cusname[],char user[],char city[],double phone,char pass[],char sex[])
{
  if(connection())
  {

        strcpy(c_name,cusname);
        strcpy(d_uname,user);
        strcpy(c_city,city);
        c_phone = phone;
        strcpy(d_password,pass);
        strcpy(gender,sex);

        EXEC SQL Insert into cust_details values(:c_name,:d_uname,:c_city,:c_phone,:d_password,:gender);
        EXEC SQL COMMIT WORK RELEASE;
        cout<<"Customer Details Added Successfully"<<endl;
  }
   else
   {
         cout<<"Connection failed";
   }

}


void Customer::viewroomdetails()
{
  if(connection())
  {
         strcpy(p,"AVAILABLE");
   EXEC SQL DECLARE Curs2 CURSOR for select * from Rooms where availability=:p;
   EXEC SQL OPEN Curs2;
   if(sqlca.sqlcode <0)
  {
    cout<<"Error in Execution";
   }
   for(;;)
   {
    EXEC SQL FETCH Curs2 Into :d_room_no,:d_room_type,:d_tariff,:d_available; //declare these local var later
    if(sqlca.sqlcode == NODATAFOUND)
{ break;}
    if(sqlca.sqlcode <0)
    {
     cout<<"Error in Execute";
    }
    cout<<"***Room Details***"<<endl;
    cout<<"Room No : "<<d_room_no<<endl;//int
    cout<<"Room Type : "<<d_room_type<<endl; //varchar
    cout<<"Tariff: "<<d_tariff<<endl; //int
    cout<<"Availability : "<<d_available<<endl;//varchar
    }
    cout<<"******************"<<endl;
    EXEC SQL COMMIT WORK RELEASE;

    }
    else
    {
      cout<<"Connection failed";
    }
}

void Customer::book(int room_id)
{
if(connection())
  {
   d_room_no = room_id;
        strcpy(m,"booked");
   EXEC SQL update Rooms set availability=:m where room_No=:d_room_no;

   if(sqlca.sqlcode <0)
 {
    cout<<"Error in Execution";
   }
   EXEC SQL COMMIT WORK RELEASE;


}

 else
    {
      cout<<"Connection failed";
    }


 //Calculations.......................

  if(d_room_no>=101 && d_room_no<111)
  {
    cal=85;
  }
else  if(d_room_no>=201 && d_room_no<211)
  {
    cal=150;
  }
else
{
  cal=300;
  }

  cout<<"Your room is booked now, You have to pay $"<<cal<<"amount";

}




********************************************************************************

HMS.cpp

#include"HMS2_cu.hpp"
#include<iostream>
#include<string.h>
#include<cstring>

using namespace std;

int main()
{
  int x,ch2,ch,temp,room_no;
double p_no;
  char id[20],pwd[20],name[20],city[10],g[10];
  Customer c;
 cout<<"Welcome to G L Resorts"<<endl;
 cout<<"Press"<<endl <<"1. Login"<<endl<<"2. Register"<<endl;
 cin>>ch;
switch(ch)
{
case 1:
  {
    cout<<"Enter your registered email id"<<endl;
    cin>>id;
    cout<<"Enter your password"<<endl;
    cin>>pwd;
    temp= c.login(id,pwd);
    if(temp==0)
    {
     do
     {
       cout<<"Welcome Customer\n";
       cout<<"Press\n"<<"1. To view Rooms\n"<<"2.To Book a Room\n"<<"3.To exit\n";
       cin>>ch;
       switch(ch)
        {
        case 1:
               cout<<"\nThese are available rooms";
               c.viewroomdetails();
                break;
       case 2:
 cout<<"\nPlease enter your room no";
                cin>>room_no;
                c.book(room_no);
                break;
       default:
                ch2=0;
                break;
         }

      }while(ch2==1);



     }
  }
   break;
case 2:
    cout<<"Enter your Name "<<endl;
    cin>>name;
    cout<<"Enter your city"<<endl;
    cin>>city;
    cout<<"Enter your phone no"<<endl;
    cin>>p_no;
    cout<<"Enter your Gender"<<endl;
    cin>>g;
    cout<<"Enter your email id"<<endl;
    cin>>id;
    cout<<"Enter your password"<<endl;
    cin>>pwd;
    c.registration(name,id,city, p_no,pwd,g);
    break;

 default:
   cout<<"Enter any valid no";
   break;
}
return 0;
}
**********************************************



